# SSH Keyscan

Run [`ssh-keyscan`] into a [known hosts file] as [a pre-command hook] for use in [Buildkite] builds.

Your pipeline may be using HTTPS repositories but you also need to use SSH, so you can't rely on built-in ssh keyscan priming your known hosts file. Or maybe you're using Terraform with SSH modules. Or you're doing SSH operations against a deployment environment.

  [`ssh-keyscan`]: https://man.openbsd.org/ssh-keyscan.1
  [known hosts file]: https://man.openbsd.org/sshd.8#SSH_KNOWN_HOSTS_FILE_FORMAT
  [a pre-command hook]: https://buildkite.com/docs/agent/v3/hooks#job-lifecycle-hooks
  [Buildkite]: https://buildkite.com

## Example

```yaml
steps:
- plugins:
  - sj26/ssh-keyscan:
      host: github.com
  command: ssh clone git@github.com:buildkite/bash-example.git
```

## Choosing known hosts location

By default, the host key will be scanned into `~/.ssh/known_hosts`.

You can scan into a different location usign the `known_hosts_path` parameter:

```yaml
steps:
- plugins:
  - sj26/ssh-keyscan:
      host: github.com
      known_hosts_path: /etc/ssh/ssh_known_hosts
```

## Caveats

ssh-keyscan will always run, and always append to the known hosts file, even if the host entry already exists. This may be solved in a future version.

Only one host can be scanned. You can add this plugin multiple times as a workaround. Multiple hosts may be supported in a future version.
