## pytest contributions

This week, I continued working on pytest by investigating two issues. I plan to investigate a third issue next.

### [Issue #13693](https://github.com/pytest-dev/pytest/issues/13693)

I reproduced the live-logging race condition on the current pytest `main` branch with both `capsys` and `capfd`. When a background thread emits a live log record, pytest temporarily suspends global and fixture capture so the log can appear on the terminal. If another thread writes to `stdout` or `stderr` during this window, its output can bypass capture and be missing from `readouterr()`.

I traced this behavior from `_LiveLoggingStreamHandler.emit()` through `CaptureManager`, `MultiCapture`, `SysCapture`, and `FDCapture`. This confirmed that capture suspension affects process-wide state: `SysCapture` changes the shared `sys.stdout` and `sys.stderr` streams, while `FDCapture` redirects the shared file descriptors using `os.dup2()`. As a result, suspending capture from one thread can affect output written by other threads.

I also reviewed the previous [PR #14564](https://github.com/pytest-dev/pytest/pull/14564) and the existing live-logging behavior. Simply removing capture suspension could change how live logs are displayed, so I posted my [findings](https://github.com/pytest-dev/pytest/issues/13693#issuecomment-5463783298) and asked the maintainers whether routing only live-log output to the saved original destination would be an acceptable direction.

### [Issue #14289](https://github.com/pytest-dev/pytest/issues/14289)

I reproduced the issue on Windows using Python 3.13.15 with both pytest 9.0.2 and the current development version `9.2.0.dev262+gfdba12e17`. It occurs with the default `fd` capture and disappears with `--capture=sys`.

I traced the failure to `logging.basicConfig()` being called before `pytest.main()`. When `logging.basicConfig()` creates the `StreamHandler`, the handler stores a reference to the current `sys.stderr` object. During pytest startup, `_windowsconsoleio_workaround()` reopens the standard streams before `FDCapture` starts, but the existing handler continues to reference the old Windows console stream. When fd capture later redirects file descriptor 2, the handle used by that retained stream can become invalid. A subsequent logging call then causes `StreamHandler.emit()` to encounter `OSError: [WinError 6] The handle is invalid`, producing the noisy logging error output.

I validated the cause with a dynamic proxy that resolves the current `sys.stderr` whenever logging writes instead of retaining a previously stored stream reference. This prevented the error while keeping fd capture enabled, providing a working user-side workaround and additional evidence for the identified cause.

Applying this proxy approach automatically would require pytest to modify logging handlers created by the application embedding `pytest.main()`, which raises a design and ownership question. I also found that a regression test using `pytester.runpython()` would not exercise this path because the helper redirects child-process output to regular files instead of a Windows console. I posted my [findings](https://github.com/pytest-dev/pytest/issues/14289#issuecomment-5472116618) and asked whether the maintainers would prefer a targeted warning and documented workaround or automatic rebinding.

### [Issue #6626](https://github.com/pytest-dev/pytest/issues/6626) — Pending
