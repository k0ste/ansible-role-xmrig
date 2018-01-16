ansible-xmrig
===============

Deploy [xmrig](//github.com/xmrig/xmrig) - Monero (XMR) CPU miner.

Requirements
---------------

* Ansible 2.2;
* jmespath;
* ArchLinux;

Extra
---------

Please note that is not possible to validate json configuration because `xmrig`
use wrong exit codes, see [xmrig#341](//github.com/xmrig/xmrig/issues/341).

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
  # User for xmrig.conf (default is 'root').
    user: 'xmrig'
  # Group for xmrig.conf (default is 'root').
    group: 'xmrig'
  # Install xmrig package or not (default is 'false').
    install_package: 'true'
  # Define xmrig service (default is 'xmrig').
    service: 'xmrig@xmrig'
  # Destination for xmrig.conf (default is '/etc/xmrig').
    conf_dest: '/etc/xmrig'
  # Package name for pacman (default is 'xmrig').
    package: 'xmrig'
  instance_settings:
  # 'cryptonight' (default) or 'cryptonight-lite'.
  - algo: 'cryptonight'
  # Algorithm variation, 0 auto select.
    av: '0'
  # True to run the miner in the background.
    background: 'false'
  # False to disable colored output.
    colors: 'true'
  # Set process affinity to CPU core(s), mask "0x3" for cores 0 and 1.
    cpu_affinity: 'null'
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
  # Use system log for output messages.
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
    - url: 'pool.minemonero.pro:5555'
      # Username for mining server.
      user: ''
      # Password for mining server.
      pass: 'x'
      # Send keepalived for prevent timeout (need pool support).
      keepalive: 'true'
      # Enable nicehash/xmrig-proxy support.
      nicehash: 'false'
  # Additional user string options supported.
    - url: 'pool.minemonero.pro:5555'
      user:
      - user_name: 'user2'
        payment_id: 'payment_id2'
        worker_name: "worker_name2"
        email: "user2@example.com"
      pass: 'x'
```
