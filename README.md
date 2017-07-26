<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [ansible-telegraf](#ansible-telegraf)
  - [Requirements](#requirements)
  - [Role Variables](#role-variables)
  - [Dependencies](#dependencies)
  - [Example Playbook](#example-playbook)
  - [License](#license)
  - [Author Information](#author-information)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# ansible-telegraf

An [Ansible](https://www.ansible.com) role to install/configure [Telegraf](https://www.influxdata.com/time-series-platform/telegraf)

## Requirements

None

## Role Variables

```yaml
---
# defaults file for ansible-telegraf
config_telegraf: true

enable_telegraf_global_tags: false

telegraf_agent_info:
  ## Collection jitter is used to jitter the collection by a random amount
  collection_jitter: 10s
  ## Run telegraf in debug mode
  debug: false
  ## Default flushing interval for all outputs
  flush_interval: 10s
  ## Jitter the flush interval by a random amount
  flush_jitter: 0s
  ## Override default hostname, if empty use os.Hostname()
  hostname: ''
  ## Default data collection interval for all inputs
  interval: 10s
  ## Telegraf will send metrics to outputs in batches of at most
  ## metric_batch_size metrics.
  metric_batch_size: 1000
  ## For failed writes, telegraf will cache metric_buffer_limit metrics for each
  ## output, and will flush this buffer on a successful write
  metric_buffer_limit: 10000
  ## If set to true, do no set the "host" tag in the telegraf agent
  omit_hostname: false
  ## Valid values are "ns", "us" (or "µs"), "ms", "s".
  precision: ''
  ## Run telegraf in quiet mode
  quiet: false
  ## Rounds collection interval to 'interval'
  round_interval: true

telegraf_basic_inputs:
  - 'diskio'
  - 'kernel'
  - 'mem'
  - 'netstat'
  - 'ntpq'
  - 'processes'
  - 'swap'
  - 'system'

telegraf_debian_file: 'telegraf_{{ telegraf_version }}-1_amd64.deb'

telegraf_dl_uri: 'https://dl.influxdata.com/telegraf/releases'

telegraf_global_tags:
  - key: 'dc'
    value: 'us-east-1'

telegraf_inputs:
  cpu:
    enabled: true
    fielddrop: 'time_*'
    ## Whether to report per-cpu stats or not
    percpu: true
    ## Whether to report total system cpu stats or not
    totalcpu: true
  disk:
    enabled: true
    ## Ignore some mountpoints by filesystem type
    ignore_fs:
      - 'devtmpfs'
      - 'tmpfs'
    ## Setting mountpoints will restrict the stats to the specified mountpoints
    mount_points:
      - '/'
      # - '/var'
  docker:
    ## Only collect metrics for these containers, collect all if empty
    container_names:
      - ''
    enabled: false
    endpoint: 'unix:///var/run/docker.sock'
    ## Whether to report for each container per-device blkio (8:0, 8:1...) and
    ## network (eth0, eth1, ...) stats or not
    perdevice: true
    ## Timeout for docker list, info, and stats commands
    timeout: 5s
    ## Whether to report for each container total blkio and network stats or not
    total: false
  elasticsearch:
    ## set cluster_health to true when you want to also obtain cluster level stats
    cluster_health: false
    enabled: false
    ## set local to false when you want to read the indices stats from all nodes
    ## within the cluster
    local: true
    servers:
      - 'localhost:9200'
  haproxy:
    enabled: false
    servers:
      - 'http://myhaproxy.com:1936/haproxy?stats'
      - 'socket:/run/haproxy/admin.sock'
  influxdb:
    enabled: false
    urls:
      - 'http://localhost:8086/debug/vars'
  memcached:
    enabled: false
    servers:
      - 'localhost:11211'
    unix_sockets:
      - '/var/run/memcached.sock'
  mongodb:
    enabled: false
    gather_perdb_stats: false
    servers:
      - 127.0.0.1:27017
  net:
    enabled: true
    ## By default, telegraf gathers stats from any up interface (excluding loopback)
    # interfaces:
    #   - 'enp0s3'
    #   - 'eth0'
    #   - 'eth1'
  powerdns:
    enabled: false
    unix_sockets:
      - '/var/run/pdns.controlsocket'
  prometheus:
    ## Use bearer token for authorization
    # bearer_token: '/path/to/bearer/token'
    enabled: false
    urls:
      - 'http://localhost:9100/metrics'
  rabbitmq:
    enabled: false
    # optional tag
    # name: 'rmq-server-1'
    nodes:
      - 'rabbit@node1'
      - 'rabbit@node2'
    password: 'guest'
    url: 'http://localhost:15672'
    username: 'guest'
  redis:
    enabled: false
    servers:
      - 'tcp://localhost:6379'
      # - 'tcp://:password@192.168.99.100'
      # - 'unix:///var/run/redis.sock'
  zfs:
    enabled: false
    kstatPath: '/proc/spl/kstat/zfs'
    kstatMetrics:
      - 'arcstats'
      - 'vdev_cache_stats'
      - 'zfetchstats'
    poolMetrics: false
  zookeeper:
    enabled: false
    servers:
      - ':2181'
      # - 10.0.0.1:2181
      # - 'localhost:2181'

telegraf_outputs:
  graphite:
    enabled: false
    ## Prefix metrics name
    prefix: ''
    servers:
      - 'localhost:2003'
      # - 'remotehost:2003'
    ## Graphite output template
    template: 'host.tags.measurement.field'
    ## timeout in seconds for the write connection to graphite
    timeout: 2
  graylog:
    enabled: false
    servers:
      - 127.0.0.1:12201
      # - 'remotehost:12201'
  influxdb:
    database: 'telegraf'
    enabled: true
    #Defines if influxdb login is required
    login_required: false
    password: 'metricsmetricsmetricsmetrics'
    retention_policy: ''
    timeout: 5s
    urls:
      - 'http://localhost:8086'
      # - 'http://remotehost:8086'
    username: 'telegraf'
    user_agent: 'telegraf'
    write_consistency: 'any'
  prometheus_client:
    enabled: false
    ## Address to listen on
    listen: ':9126'

telegraf_redhat_file: 'telegraf-{{ telegraf_version }}.x86_64.rpm'

telegraf_version: 1.3.4
```

## Dependencies

None

## Example Playbook

## License

MIT

## Author Information

Larry Smith Jr.

-   [@mrlesmithjr](https://www.twitter.com/mrlesmithjr)
-   [EverythingShouldBeVirtual](http://www.everythingshouldbevirtual.com)
-   [mrlesmithjr.com](http://mrlesmithjr.com)
-   mrlesmithjr [at] gmail.com
