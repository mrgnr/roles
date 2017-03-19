# onionbalance

This role installs and configures [OnionBalance][onionbalance] and (optionally)
[Tor hidden services][hidden-services]. OnionBalance and Tor are run in [rkt][rkt] containers.

## Prerequesites

- rkt must be [installed][rkt-install] on the target machine. You can install rkt using the
[rkt role][rkt-role].

## Variables

- `onionbalance_config` -- A YAML data structure containing the OnionBalance
[configuration][onionbalance-config-file]. The contents of this data structure will be dumped to
the OnionBalance configuration file on the target host.

- `onionbalance_config_dir` -- Path of the OnionBalance configuration directory on the target host.

- `onionbalance_data_dir` -- Path of the tor data directory on the target host.

- `onionbalance_hashed_control_password` -- The [hashed password][hashed-control-pw] used by the
tor control socket.

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

## Example configuration

```
onionbalance_config:
  TOR_CONTROL_PASSWORD: "hqZwhy93DkWtn8xh04DvPMi8UAnCc5cQ"
  services:
    - instances:
      - address: "nwvojuhvwu7ecyid"
        name: "foo-0"
      - address: "yqwxbunogc4sb6hz"
        name: "foo-1"
      key: "t5mujxp7fa2m55qx.key"

onionbalanced_hashed_control_password: "16:E17ABDF94A9AEA5060A805A6ECA6D15738FD71818EF5D32DEA57DE2482"

onionbalance_private_keys:
  - name: "t5mujxp7fa2m55qx"
    key: |
      -----BEGIN RSA PRIVATE KEY-----
      MIICXAIBAAKBgQDiqD7xhGaqC1TXQLmMI8lIEZSVK4Tp5PkVvdPVDymhvVXdu5qB
      hVILVrE37M+R1KGrSAP3AQ97QZYPBW/m2OliaXSCSGQHgu7Q1yKOEmAQ1V2ciXPF
      wsz7ijReaggljysogsmtsqxmhlte3GA0goP9plDr+H/pIB9XwL8zSv6XgQIDAQAB
      AoGBAM4DfrKnVWlZw1OjUQM/w8Ptts+fLsApjv1j/Ra7IWwRW+qeimEPfPMxaQMc
      C87RJeE5I+Fu5VNy2aHtnziEEBvdOI06V1Vl3/0FfdtsWQ0kdDMrgR+MurwsDd36
      S1m0csMYMxQ6D/z4s9oeIuCFgouvMY9GMGeD0ZuynY9i50HBAkEA79CPsm0drBHz
      bl4ahKBHrPgSTlZrZmiXl892MNZGAmJ5Kwwh2oM4tPWN7ezfqMTLN1d9XSBaFHYY
      dnp8WAH18wJBAPH0WpwiKmsooLzikKoygiM71muSwCM0+a5G4kZbY1347Fsm55W4
      CSFuz9Kda15ObC+WfZBN0/7VDPA4MP/RFbsCQDPVO0nQZcpsMtZXBppF3lgXYjWG
      Xj5LOwC3+Y7CsW0Qhan1PFfzZs1OCbg0K39Z0aaLhXAcbvvfLphlDv0ip1sCQFl4
      /gTc0Yjc9kvDELIPiXZUC1+uXeTnEymyRryz0NQQV/8BLQR9kMrPOoTs96ZhI7qZ
      UQeK8Ek9KdKvRNdkzz8CQDAfnpisgWG6Zwdn8kRRn0xzuDzA757NH1see+47fG5e
      +HajW7sxslZcOwz0IbhFl4TqloTD36KJTZ90IdcFMSw=
      -----END RSA PRIVATE KEY-----

onionbalance_hs_config:
  - service_name: "foo-0"
    data_dir: "/var/lib/foo-0"
    torrc: "/etc/onionbalance/foo-0/torrc"
    port: 80
    target: "192.168.0.10:80"
    hostname: "nwvojuhvwu7ecyid.onion"
    private_key: |
      -----BEGIN RSA PRIVATE KEY-----
      MIICXgIBAAKBgQDqxGEeDgggElb6AD84jj4YQT9uZPde0gsT7zJobdHabj5HApDv
      uoj2Zi2hats293YPRerKch1HDJy+2eV02QGHuTZ339A2n2yJC1WjbF78kckzBWFl
      R7sqxc/5RCg9G6eM/5hCcolChA2eZB3lx/7GRmyXlVHy8+ygQE5ba//q0wIDAQAB
      AoGBAN97sAO/UcbODpQQdh5tcfj+69Y4MS/HfZotYuv8Vv+7YHeSnsxG4yjEHf4C
      TcomifGCGj87oFlJTVF09bRk+8kYIl8Hd6H6RJEpGujZZSbOzsqGMOLiL3/Q7HpG
      KnrdNRMeVW6YU3EzWqGGwzgYvYXtnuBXOHUZAAII12EM2pjxAkEA8fnvmI6rmVWN
      aNPjqBONPRh2GyaiycjvqKdUJshSxk/S9FKT4bpIw5BM7iE6+PAFbDV5HrFiQhgE
      jcbyMKcNyQJBAPhffA4PUtcWRGbt0ztt4dt/X1lPvotQRvRi0HG3pGmSolMYn18n
      IqjkNkDsMD+BU4Uzk/Gh0ts2ik25AFbakbsCQQDGmps6na7eNUfRSEhMRW/hO1iE
      xFtMPy3lQSFii3zU4+ODQNu7o4fha2iY8pFSjL4GqIT22iSJluj17NoPcK1xAkA8
      Qg466wTSIhjeT/zbgkE1m6Vqaap06jkMuZyQulktM+Il/udLkpcaGqP/BE6AWcQF
      oQnXqccaYBUV3jhy2fxZAkEA0/yTLkNokUH5VEdmIYM4uTgMyXdYkZUGhNUH1Ez7
      nPLayB4u1dgYbPmIJCvXS6Kpv5PF32IXSMfmdSF0PzB20w==
      -----END RSA PRIVATE KEY-----
  - service_name: "foo-1"
    data_dir: "/var/lib/foo-1"
    torrc: "/etc/onionbalance/foo-1/torrc"
    port: 80
    target: "192.168.0.20:80"
    hostname: "yqwxbunogc4sb6hz.onion"
    private_key: |
      -----BEGIN RSA PRIVATE KEY-----
      MIICXAIBAAKBgQCRgTphOmSzWor/DTEVkZ6AgqobZhKe3q63LM0NmdXbkXSY+PVX
      dMq4j1RVqkA/OgZ/IVXQLd1+UYkix7oUG44Jz3S6xXYfIRyKK8WB8/FqZ7QuZwJ1
      Vz8YRflxpS5j4WoILo0iI7cFj2EF7e/PiLxUWl4SQU/u139uLSpJkOFg8QIDAQAB
      AoGAToIcntNj/EXxU3apskqU7CAUap4jk+bw/FLG/PyxIDyWXeeOcTbKHtTvGx22
      dqb3VGcHJ0FoDj0uMj7zzt+jPiVHsd56V9Xs6PpBoOZ6M/jlZwtWkLQfPYac/3gS
      8r1fWkKoBmZ1kkwie01dmW/XFFuuxMcE2rLlGHxrToaEJ/UCQQC26LzQbut4SQse
      dLoOx5P/6Ze4PxH742ASZ+TN2TbVfV+TfBU3g9bAxNXc5aiXEmFV4LEvLQLU4tn5
      1nZrJBKTAkEAy6YY1E+POWN8VgEOnGeRA1TuSoa/sXPB8m/r6yFKfrjrdM2c8axc
      e+J3fmMzaE2MdKUbWtqQYzCIOszbaLrc6wJBAIWEn1AHqBvGNjelPaxMQ90rx3TX
      lWkqMZc9/+fECCMPwhUHHvXHZ5yQEw2NF+Qvpp3px22IjeiZMEUQKKFNU8ECQGRC
      hMzZ0nB1izwoTxIvZtRWFu74Ah4SGHUMJwDepfdXgQxDQjY1Hl8bcqr1mdSLAVBY
      DOyg2B8NQLR6MLcR8DsCQCALgP1afmgNhSk8Zki5r5TeCJDLu0dIuEMAmyNKpTTd
      6r1j8xIY0VoUfKHazO2syx4/R2gsaxnhKR4dDvR3svo=
      -----END RSA PRIVATE KEY-----
```

## Notes

The [`onionbalance-config`][onionbalance-config] tool is useful for generating some of the
necessary configuration, including the main OnionBalance configuration, `.onion` addresses, and
private keys.


[onionbalance]: https://github.com/DonnchaC/onionbalance
[hidden-services]: https://www.torproject.org/docs/hidden-services
[rkt]: https://github.com/coreos/rkt
[rkt-install]: https://coreos.com/rkt/docs/latest/distributions.html
[rkt-role]: https://github.com/mrgnr/roles/tree/master/rkt
[onionbalance-config-file]: https://onionbalance.readthedocs.io/en/latest/running-onionbalance.html#configuration-file-format
[hashed-control-pw]: https://www.torproject.org/docs/tor-manual.html.en#HashedControlPassword
[rkt-fetch]: https://coreos.com/rkt/docs/latest/subcommands/fetch.html
[rkt-trust]: https://coreos.com/rkt/docs/latest/subcommands/trust.html
[onionbalance-config]: https://onionbalance.readthedocs.io/en/latest/onionbalance-config.html
