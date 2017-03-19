# onionbalance

This role installs and configures [OnionBalance][onionbalance] and (optionally)
[Tor hidden services][hidden-services]. OnionBalance and Tor are run in [rkt][rkt] containers.

## Prerequesites

- rkt must be [installed][rkt-install] on the target machine. You can install rkt using the
[rkt role][rkt-role].

## Variables

- `onionbalance_config` -- A YAML data structure containing the OnionBalance
[configuration][onionbalance-config]. The contents of this data structure will be dumped to the
OnionBalance configuration file on the target host.

- `onionbalance_config_dir` -- Path of the OnionBalance configuration directory on the target host.

- `onionbalance_data_dir` -- Path of the tor data directory on the target host.

- `onionbalance_hashed_control_password` -- The [hashed password][hashed-control-pw] use by the tor
control socket.

- `onionbalance_hs_config` -- A list of hashes where each hash specifies a hidden service. Each hash
should specify the following keys: `service_name`, `data_dir`, `torrc`, `port`, `target`,
`hostname`, and `private_key`.

- `onionbalance_private_keys` -- A list of OnionBalance private keys.

- `onionbalance_rkt_fetch_images` -- A list of hashes specifying rkt images to fetch. Each hash must
contain an `image` key specifying the name of the image and may contain `signature` and
`pull_policy` keys, which correspond to paramters taken by the [`rkt fetch`][rkt-fetch] command.

- `onionbalance_rkt_trust` -- A list of hashes specifying domains to trust when downloading rkt
images. Each hash may contain `root`, `prefix`, and `location` keys, which correspond to parameters
taken by the [`rkt trust`][rkt-trust] command.

- `onionbalance_tor_user` -- The name of the tor user to be created on the target host.

- `onionbalance_tor_user_uid` -- The uid of the tor user to be created on the target host.


[onionbalance]: https://github.com/DonnchaC/onionbalance
[hidden-services]: https://www.torproject.org/docs/hidden-services
[rkt]: https://github.com/coreos/rkt
[rkt-install]: https://coreos.com/rkt/docs/latest/distributions.html
[rkt-role]: https://github.com/mrgnr/roles/tree/master/rkt
[onionbalance-config]: https://onionbalance.readthedocs.io/en/latest/running-onionbalance.html#configuration-file-format
[hashed-control-pw]: https://www.torproject.org/docs/tor-manual.html.en#HashedControlPassword
[rkt-fetch]: https://coreos.com/rkt/docs/latest/subcommands/fetch.html
[rkt-trust]: https://coreos.com/rkt/docs/latest/subcommands/trust.html
