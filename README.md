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
- `nncp_reass_schedule` (string): How often to run `nncp-reass` to reassemble
  any chunked packets in the spool. You can test this time specification with
  `systemd-analyze calendar` as well. Defaults to `hourly`.
- `nncp_toss_interval_seconds` (integer): How often, in seconds, to run
  `nncp-toss`. Corresponds to the `nncp-toss` `-cycle` option. Defaults to `300`
  (5 minutes).
- `nncp_daemon_enabled` (bool): Whether to enable the `nncp-daemon.service`
  unit that will listen to inbound connections and process packets coming in.
  Defaults to `true`.


## Credits

The original service and logrotate files this role is based on originate from
the Debian NNCP package maintained by John Goerzen on
[salsa.debian.org](https://salsa.debian.org/go-team/packages/nncp).


<!-- vim: set textwidth=80 sw=2 ts=2: -->
