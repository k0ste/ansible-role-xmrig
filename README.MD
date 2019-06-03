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
  - name: 'cpu0'
# Mandatory variable: miner type: 'cpu' for cpu miner, 'amd' for gpu miner.
    type: 'cpu'
# 'cryptonight' (default) or 'cryptonight-lite'.
    algo: 'cryptonight'
# 'true' or 'auto' - will try use proper assembly by CPUID information. This is
# default value. 'false' or 'none' - feature disabled. 'intel' best for Intel
# CPUs starting from Ivy Bridge/Sandy Bridge, 'ryzen' best for AMD Ryzen CPUs.
    asm: 'auto'
# Algorithm variation, 0 auto select.
    av: '0'
# True to run the miner in the background.
    background: 'false'
# False to disable colored output.
    colors: 'false'
# Set process affinity to CPU core(s) (defatul is 'null').
    cpu_affinity: '0x7E'
# Set process priority (0 idle, 2 normal to 5 highest).
    cpu_priority: 'null'
# Donate level, mininum 1%.
    donate_level: '5'
# Use huge pages or not.
    huge_pages: 'false'
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
# User agent (HTTP).
    user_agent: 'null'
# Watch for config (?).
    watch: 'false'
# Number of miner threads.
    threads: 'null'
# https://github.com/xmrig/xmrig/wiki/API
    api:
    - port: '0'
      access_token: 'null'
      worker_id: 'null'
      ipv6: 'false'
      restricted: 'true'
    pools:
# URL of mining server.
    - url: 'monero.napaster.name:14444'
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
# Algorithm PoW variant, '-1' for autodetection.
      variant: '-1'
# Enable TLS support.
      tls: 'false'
# Optionally verify pool certificate fingerprint.
      tls_fingerprint: 'null'
  - name: 'cpu1'
    type: 'cpu'
    algo: 'cryptonight-lite'
    asm: 'auto'
    av: '0'
    background: 'false'
    colors: 'false'
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
    user_agent: 'null'
    watch: 'false'
    threads: 'null'
    api:
    - port: '0'
      access_token: 'null'
      worker_id: 'null'
      ipv6: 'false'
      restricted: 'true'
    pools: "{{ xmrig_pools }}"
# AMD GPU miner
  - name: 'opencl0'
    type: 'amd'
    algo: 'cryptonight-lite'
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
    safe: 'false'
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
    - port: '0'
      access_token: 'null'
      worker_id: 'null'
      ipv6: 'false'
      restricted: 'true'
    pools: "{{ xmrig_pools }}"
```
