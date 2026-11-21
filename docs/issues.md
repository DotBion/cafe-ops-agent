# Build issues

| Issue | Impact | Resolution |
| --- | --- | --- |
| The workspace's HTTP proxy uses SOCKS | The service failed to initialize the HTTP client without the proxy extra. | Declared the HTTPX SOCKS dependency and restarted the service successfully. |
| A timeline event did not narrow to the error variant in TypeScript | The production frontend build stopped. | Narrowed the event by its discriminator and reran the build. |
| Local Chromium could not create its required socket in the workspace | Local headless browser checks could not launch. | Added a browser check to the GitHub-hosted build workflow. |
