ansible-xmrig
===============

Deploy [xmrig](//github.com/xmrig/xmrig) - High Perf CryptoNote CPU Miner
(Monero, Aeon).

Requirements
---------------

* Ansible 2.2;
* xmrig 2.4.5+
* jmespath;
* Linux systemd distro (ArchLinux 100% supported);

Extra
---------

~~Please note that is not possible to validate json configuration because `xmrig`
use wrong exit codes, see [xmrig#341](//github.com/xmrig/xmrig/issues/341).~~
Option `--dry-run` supported since `xmrig` 2.4.5.

Role will supported multi `xmrig` instances. E.g. you can run two instances on
different cpu's, for example one instance for `monero` another one for `aeon`.

Example configuration
-----------------------

```yaml
---
xmrig:
- role_settings:
  # Enable xmrig service (default is 'false').
  - enable: 'true'
  # Restart xmrig after successful deploy (default is 'false').
    restart: 'true'
  # Install xmrig package or not (default is 'false').
    install_package: 'true'
  instance_settings:
  - name: 'cpu0'
    # 'cryptonight' (default) or 'cryptonight-lite'.
    algo: 'cryptonight'
    # Algorithm variation, 0 auto select.
    av: '0'
    # True to run the miner in the background.
    background: 'false'
    # False to disable colored output.
    colors: 'true'
    # Set process affinity to CPU core(s) (defatul is 'null').
    cpu_affinity: '0x7E'
    # Set process priority (0 idle, 2 normal to 5 highest).
    cpu_priority: 'null'
    # Donate level, mininum 1%.
    donate_level: '5'
    # Log all output to a file, example: "/var/log/xmrig/xmrig.log".
    log_file: 'null'
    # Maximum CPU usage for automatic mode, usually limiting factor is CPU cache
    # not this option.
    max_cpu_usage: '75'
    # Print hashrate report every N seconds.
    print_time: '60'
    # Number of times to retry before switch to backup server.
    retries: '5'
    # Time to pause between retries.
    retry_pause: '5'
    # Set to 'true' to safe adjust threads and av settings for current CPU.
    safe: 'false'
    # Use system log for output messages (default is 'false'). Set to 'true'
    # for output log to systemd journald.
    syslog: 'false'
    # Number of miner threads.
    threads: 'null'
    # https://github.com/xmrig/xmrig/wiki/API
    api:
    - port: '0'
      access_token: 'null'
      worker_id: 'null'
    pools:
    # URL of mining server.
    - url: 'xmr.napaster.name:15555'
      # Username for mining server.
      user: '43kwWi8seNj1arEEKkbS95jAQRdPuY6hmiKqwS3wirGgNG5xPqUBxJvj214k1w48rB8Hzht9ZpUWcb2AWxmCL41tCH11mjh'
      # Password for mining server.
      pass: 'x'
      # Send keepalived for prevent timeout (need pool support).
      keepalive: 'true'
      # Enable nicehash/xmrig-proxy support.
      nicehash: 'false'
  - name: 'cpu1'
    algo: 'cryptonight-lite'
    av: '0'
    background: 'false'
    colors: 'true'
    cpu_affinity: '0x7E00'
    cpu_priority: 'null'
    donate_level: '5'
    log_file: 'null'
    max_cpu_usage: '75'
    print_time: '60'
    retries: '5'
    retry_pause: '5'
    safe: 'false'
    syslog: 'false'
    threads: 'null'
    api:
    - port: '0'
      access_token: 'null'
      worker_id: 'null'
    pools:
    # Additional user string options supported.
    - url: 'aeon.napaster.name:43335'
      user:
      - user_name: 'Wmt7Dx71mmZFaKNMxzD12KXKv1D7moHhKEKV9aZxZ3gAYF8NqLygY23T1heASC2fnxAYcfKW7x2xN9UCtKJBPusg1GM9ZGEcD'
        payment_id: 'payment_id2'
        worker_name: "worker_name2"
        email: "user2@example.com"
      pass: 'x'
```
