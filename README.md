# agentgolf CLI

`agentgolf` is the command-line interface of [agent.golf](https://agent.golf). It connects your coding-agent harness to Uplink with your personal publishing key, and links your agent sessions to GitHub pull requests.

This repository holds the CLI [releases](https://github.com/agentgolf/cli/releases). Full documentation: [docs.agent.golf/cli/overview](https://docs.agent.golf/cli/overview).

The CLI runs on Linux (`amd64` or `arm64`, glibc 2.34 or later) and on macOS on Apple silicon. It needs the credential store of the operating system.

## Install

1. Download the archive of your system and `SHA256SUMS` from the [latest release](https://github.com/agentgolf/cli/releases/latest). Put the version in place of `<version>`:

   ```sh
   curl -fsSLO https://github.com/agentgolf/cli/releases/download/v<version>/agentgolf-linux-amd64.tar.gz
   curl -fsSLO https://github.com/agentgolf/cli/releases/download/v<version>/SHA256SUMS
   ```

   For another system, replace `linux-amd64` with `linux-arm64` or `darwin-arm64`.

2. Check the archive. On macOS, use `shasum -a 256 --check --ignore-missing SHA256SUMS`.

   ```sh
   sha256sum --check --ignore-missing SHA256SUMS
   ```

3. Extract the binary and put it on your `PATH`:

   ```sh
   tar -xzf agentgolf-linux-amd64.tar.gz
   sudo install -m 0755 agentgolf /usr/local/bin/agentgolf
   agentgolf --version
   ```

The notes of each release also show the `cosign` command that checks the signature of `SHA256SUMS`.

## Log in

Set the API address. Add this line to the profile of your shell:

```sh
export AGENTGOLF_API_BASE_URL=https://api.agent.golf
```

Then log in with device authorization:

1. Run `agentgolf login`.
2. The CLI shows a URL and a code, and opens the URL in your browser.
3. On the **Approve a Device** page, make sure that **Device Code** shows the same code.
4. Select your **Workspace**, then select **Approve**.
5. The terminal shows `Logged in.`

The code expires after 5 minutes. The CLI keeps its refresh token in the credential store of the operating system. To log out, run `agentgolf logout`. See [Log In](https://docs.agent.golf/cli/login).

## Commands

| Command                                                              | Result                                                                                         |
| -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `agentgolf login`                                                    | Sign in and save the login.                                                                    |
| `agentgolf logout`                                                   | Remove the saved login.                                                                        |
| `agentgolf harness setup <claude-code\|codex\|omp>`                  | Create a personal publishing key and write the settings of the harness.                        |
| `agentgolf attribute <session-id> <pull-request-url>`                | Link one agent session to one GitHub pull request.                                             |
| `agentgolf attribution evidence --session <uuid>`                    | Show the attribution records of one session.                                                   |
| `agentgolf attribution evidence --pull-request <url>`                | Show the attribution records of one pull request.                                              |
| `agentgolf attribution repositories`                                 | List the repositories that attribution covers in your workspace.                               |
| `agentgolf attribution audit --since <time> --before <time>`         | Compare automatic links with manual links and pull-request authors. Times are UTC `YYYYMMDDHHmmss`. |

`agentgolf harness setup` flags:

| Flag                        | Result                                                                                             |
| --------------------------- | -------------------------------------------------------------------------------------------------- |
| `--endpoint <url>`          | Use the OTLP listener of a Self-Hosted Uplink instance. Without it, the command uses Cloud Uplink. |
| `--allow-insecure-endpoint` | Accept an `http` endpoint on a host that is not loopback.                                          |
| `--name <name>`             | The name of the key.                                                                               |
| `--replace`                 | Change settings that already have other values.                                                    |
| `--print`                   | Print the settings and write no file.                                                              |

Run `agentgolf <command> --help` for the help of each command. See [CLI Commands](https://docs.agent.golf/cli/commands).

## Documentation

[docs.agent.golf/cli/overview](https://docs.agent.golf/cli/overview)
