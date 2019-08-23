# ansible-xmrig

Deploy [xmrig](//github.com/xmrig/xmrig) - High Perf CryptoNote Miner
(Monero, Aeon).

## Requirements

* Ansible 2.8+;
* xmrig 2.4.5+
* Linux systemd distro (ArchLinux 100% supported);

# Extra

~~Please note that is not possible to validate json configuration because
`xmrig` use wrong exit codes, see
[xmrig#341](//github.com/xmrig/xmrig/issues/341).~~

Option `--dry-run` supported since `xmrig` 2.4.5.

Role will supported multi `xmrig` instances. E.g. you can run two instances on
different cpu's, for example one instance for `monero` another one for `aeon`.

Also on same rig you can run `xmrig-amd` instances for OpenCL computing.

# Example configuration

```yaml
---
# Template user settings
xmrig_wallet: 'Wmt7Dx71mmZFaKNMxzD12KXKv1D7moHhKEKV9aZxZ3gAYF8NqLygY23T1heASC2fnxAYcfKW7x2xN9UCtKJBPusg1GM9ZGEcD'
xmrig_difficulty: '10000'

# Template pool settings
xmrig_pools:
- url: 'aeon.napaster.name:7777'
  user: "{{ xmrig_wallet + '.' + xmrig_difficulty }}"
  pass: 'x'
  rig_id: 'null'
  keepalive: 'true'
  nicehash: 'false'

# Role settings
xmrig:
# Enable xmrig service (default is 'false').
- enable: 'true'
# Restart xmrig after successful deploy (default is 'false').
  restart: 'true'
# Install xmrig package or not (default is 'false').
  install_package: 'true'
# 'present' (do nothing if package is already installed) or 'latest' (always
#  upgrade to last version). Default is 'present'.
  package_state: 'latest'
  xmrig_settings:
# Mandatory variable.
  - name: 'cpu'
# Mandatory variable: miner type: 'cpu' for cpu miner, 'amd' for gpu miner.
    type: 'cpu'
# CPU settings (since xmrig 3.0).
    cpu:
# 'true' or 'auto' - will try use proper assembly by CPUID information. This is
# default value. 'false' or 'none' - feature disabled. 'intel' best for Intel
# CPUs starting from Ivy Bridge/Sandy Bridge, 'ryzen' best for AMD Ryzen CPUs.
    - asm: 'auto'
# This option possible values is range from 1 to 5.
      intensity: '2'
# Mining threads priority, value from 1 (lowest priority) to 5 (highest
# possible priority). Default value 'null' means miner don't change threads
# priority at all.
      priority: 'null'
# Force enable ('true') or disable ('false') hardware AES support. Default
# value 'null' means miner autodetect this feature. Usually don't need change
# this option, this option useful for some rare cases when miner can't detect
# hardware AES, but it available. If you force enable this option, but your
# hardware not support it, miner will crash.
      hw_aes: 'null'
# Enable ('true') or disable ('false') huge pages support, by default 'true'.
      huge_pages: 'false'
# Number of miner threads.
      threads: 'null'
# True to run the miner in the background.
    background: 'false'
# False to disable colored output.
    colors: 'false'
# Set process affinity to CPU core(s) (defatul is 'null').
    affinity: '0x7E'
# Donate level, mininum 1%.
    donate_level: '5'
# '0' - donate over proxy disabled, only direct connections allowed.
# '1' - miner will try use proxy first with failback to direct connections.
# '2' - direct connections disabled.
# Default is '1'.
    donate_over_proxy: '1'
# Log all output to a file, example: "/var/log/xmrig/xmrig.log".
    log_file: 'null'
# Print hashrate report every N seconds.
    print_time: '60'
# Number of times to retry before switch to backup server.
    retries: '5'
# Time to pause between retries.
    retry_pause: '5'
# Use system log for output messages (default is 'false'). Set to 'true'
# for output log to systemd journald.
    syslog: 'false'
# User agent (HTTP).
    user_agent: 'null'
# Watch for config (?).
    watch: 'false'
    api:
# Miner ID, if not set created automatically.
    - id: 'null'
# Optional worker name, if not set will be detected automatically.
      worker_id: 'null'
    http:
# Enable (true) or disable (false) HTTP API.
    - enabled: 'true'
# Host for incoming connections http://<host>:<port>, to allow connections from
# all interfaces use 0.0.0.0 (IPv4) or :: (IPv4+IPv6).
      host: '127.0.0.1'
# Port for incoming connections http://<host>:<port>, zero port is valid option
# and means random port.
      port: '0'
# Bearer access token to secure access to API. Miner support this token only via
# 'Authorization' header.
      access_token: 'null'
# Use 'false' to allow remote configuration.
      restricted: 'true'
# RandomX algo settings.
    randomx:
# Disable per numa allocation if not enough RAM (default is enabled).
    - numa: 'false'
      init: '-1'
    pools:
# URL of mining server.
    - url: 'monero.napaster.name:14444'
# Algorithm PoW variant.
      algo: 'cn/r'
# Username for mining server.
      user: '43kwWi8seNj1arEEKkbS95jAQRdPuY6hmiKqwS3wirGgNG5xPqUBxJvj214k1w48rB8Hzht9ZpUWcb2AWxmCL41tCH11mjh'
# Password for mining server.
      pass: 'x'
# Rig identifier for pool-side statistics (needs pool support).
      rig_id: 'null'
# Send keepalived for prevent timeout (need pool support).
      keepalive: 'true'
# Enable nicehash/xmrig-proxy support.
      nicehash: 'false'
# Enable TLS support.
      tls: 'false'
# Optionally verify pool certificate fingerprint.
      tls_fingerprint: 'null'
# Connect to daemon (monerod) instead pool?
      daemon: 'false'
# AMD GPU miner
  - name: 'opencl0'
    type: 'amd'
    autosave: 'false'
# Enable OpenCL cache or not.
    cache: 'true'
# OpenCL platform index.
    opencl_platform: '0'
    background: 'false'
    colors: 'false'
    donate_level: '5'
    log_file: 'null'
    print_time: '60'
    retries: '5'
    retry_pause: '5'
    syslog: 'false'
    user_agent: 'null'
    watch: 'false'
    threads:
# GPU index number usually starts from 0.
    - index: '0'
# Number of parallel GPU threads (nothing to do with CPU threads).
      intensity: '416'
# Number of local GPU threads (nothing to do with CPU threads), default value 8.
      worksize: '8'
# Switch memory pattern used for the scratchpad memory.
# '0' or 'false' use a contiguous block of memory per thread.
# '1' or 'true' use 16 byte contiguous memory per thread, the next memory block
# has offset of intensity blocks.
# '2' - chunked memory, chunk size is controlled by 'mem_chunk', intensity must
# be a multiple of worksize. For cryptonight variant 2 value '1' should never
# used, on NVIDIA platform only value 0 available.
      strided_index: '2'
# Range from '0' to '18': set the number of elements (16 byte) per chunk. This
# value is only used if 'strided_index' equal to 2. Element count is computed
# with the equation: '2' to the power of 'mem_chunk' e.g. 4 means a chunk of
# 16 elements (256 byte).
      mem_chunk: '2'
# Allow to control how often the POW main loop is unrolled, valid range from
# '1' to '128' - for most OpenCL implementations it must be a power of two.
      unroll: '8'
# Compatibility enable/disable the automatic guard around compute kernel which
# allows to use a intensity which is not the multiple of the worksize. If you
# set false and the intensity is not multiple of the worksize the miner can
# crash, in this case set the intensity to a multiple of the worksize or
# activate 'comp_mode'.
      comp_mode: 'true'
# This will affine the thread to a CPU. This can make a GPU miner play along
# nicer with a CPU miner. Number or false, default value false.
      affine_to_cpu: 'false'
    - index: '1'
      intensity: '416'
      worksize: '8'
      strided_index: '2'
      mem_chunk: '2'
      unroll: '8'
      comp_mode: 'true'
      affine_to_cpu: 'false'
    api:
    - id: 'null'
      worker_id: 'null'
    http:
    - enabled: 'true'
      host: '127.0.0.1'
      port: '0'
      access_token: 'null'
      restricted: 'true'
    pools: "{{ xmrig_pools }}"
```
