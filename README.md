# NNCP Ansible role

This role deploys the excellent [NNCP](https://nncp.mirrors.quux.org/) to
Debian 12+ hosts.

The role is currently aimed at configuring server-side installations of
`nncp-daemon` and `nncp-caller` and will configure both as system services.

`/var/lib/nncp` is used for all data, mainly the NNCP spool. `/var/log/nncp/`
is set up as the log directory, with `caller.log` and `daemon.log` being used
in the default configuration for `nncp-caller` and `nncp-daemon`, respectively.

Aside from the daemon processes, the `nncp-clean.timer` timer is configured to
regularly clean up the spool.

This role makes no effort to replicate the NNCP [configuration
file](https://nncp.mirrors.quux.org/Configuration.html) over YAML variables.
Instead, the entire host configuration must be specified in the
`nncp_configuration` variable.

Extensive use of sandboxing via unit file options is made.


## Configuration

### Mandatory variables

- `nncp_configuration` (string): NNCP configuration. See the [official
  documentation](https://nncp.mirrors.quux.org/Configuration.html) for guidance.
  The `nncp-cfgnew` command will generate a default configuration file for you.

### Optional variables

- `nncp_cleanup_older_than` (string): Clean up `part`, `seen`, `hdr` and `area`
  spool files that are older than the specified time. Defaults to `30d`.
- `nncp_cleanup_tmp_older_than` (string): Clean up `tmp` spool files that are
  older than the specified time. Defaults to `30d`.
- `nncp_cleanup_schedule` (string): How often to run the `nncp-clean` service to
  perform the cleanups above. You can test your time specifications with
  `systemd-analyze calendar`. Defaults to `weekly`.


## Credits

The original service and logrotate files this role is based on originate from
the Debian NNCP package maintained by John Goerzen on
[salsa.debian.org](https://salsa.debian.org/go-team/packages/nncp).


<!-- vim: set textwidth=80 sw=2 ts=2: -->
