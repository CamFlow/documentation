# Configuring provenance capture

The CamFlow kernel configuration can be found at `/etc/camflow.ini`. An example configuration follows:

``` INI
[provenance]
;unique identifier for the machine, use hostid if set to 0
machine_id=0
;enable provenance capture
enabled=true
;record provenance of all kernel object
all=false
node_filter=directory
node_filter=inode_unknown
node_filter=char
node_filter=envp
; propagate_node_filter=directory
; relation_filter=sh_read
; relation_filter=sh_write
; propagate_relation_filter=write

[compression]
; enable node compression
node=true
edge=true
duplicate=false

[file]
;set opaque file
opaque=/usr/bin/bash
;set tracked file
;track=/home/thomas/test.o
;propagate=/home/thomas/test.o

[ipv4−egress]
;propagate=0.0.0.0/0:80
;propagate=0.0.0.0/0:404
;record exchanged with local server
;record=127.0.0.1/32:80

[ipv4−ingress]
;propagate=0.0.0.0/0:80
;propagate=0.0.0.0/0:404
;record exchanged with local server
;record=127.0.0.1/32:80


[user]
;track=vagrant
;propagate=vagrant
;opaque=vagrant

[group]
;track=vagrant
;propagate=vagrant
;opaque=vagrant

[secctx]
;track=system_u:object_r:bin_t:s0
;propagate=system_u:object_r:bin_t:s0
;opaque=system_u:object_r:bin_t:s0
```

Some information about these parameters can be gleaned by perusing the header files in the kernel module.
See [here](https://github.com/CamFlow/camflow-dev/tree/master/security/provenance/include).

See [there](https://github.com/CamFlow/camflow-dev/blob/master/docs/RELATIONS.md) for a complete list of edge types, and [here](https://github.com/CamFlow/camflow-dev/blob/master/docs/VERTICES.md) for a complete list of vertex types.

# Configuring provenance publication

The file `/etc/camflowd.ini` allows you to modify the configuration of the provenance publication service. The service needs to be restarted for a new configuration to be applied (through `systemctl restart camflowd.service`).
You can specify the `output` sink and associated parameters.
`camflowd` also support a number of output `formats`.

## Sample configuration

``` INI
[general]
; output=null
; output=mqtt
; output=unix_socket
; output=fifo
output=log

format=w3c
;format=spade_json

[log]
path=/tmp/audit.log

[mqtt]
address=m12.cloudmqtt.com:17065
username=camflow
password=test
; message delivered: 0 at most once, 1 at least once, 2 exactly once
qos=2

[unix]
address=/tmp/camflowd.sock

[fifo]
path=/tmp/camflowd-pipe
```

## Configuration parameters

Following is a list of the parameters and their effects, broken down by section.

### general

| parameter | description |
|-----------|-------------|
| output    | where to publish provenance records? |
| format    | in what format to publish provenance records? |

#### output

| value | effect |
|-------|--------|
| null  | discard provenance records |
| mqtt  | publish to an MQTT service |
| unix\_socket | publish to a UNIX socket |
| fifo  | publish to a named pipe ("FIFO") |
| log   | publish to a log file |

#### format

| value | effect |
|-----------|-------------|
| w3c       | use [w3c PROV-JSON](./w3c.md) format |
| spade\_json | use [SPADE JSON](https://github.com/ashish-gehani/SPADE/wiki/Reporting-provenance-using-JSON) format |

### log

If `output=log`, these parameters take effect.

| parameter | description |
|-----------|-------------|
| path      | publish to this file |

### mqtt

If `output=mqtt`, these parameters take effect.

| parameter | description |
|-----------|-------------|
| address   | MQTT broker |
| username  | credentials |
| password  | credentials |
| qos       | delivery guarantees |

#### qos

For each record, publish to broker...

| value | effect |
|-------|--------|
| 0     | at most once  |
| 1     | at least once |
| 2     | exactly once  |

### unix\_socket

If `output=unix_socket`, these parameters take effect.

| parameter | description |
|-----------|-------------|
| address   | publish to this socket |

### fifo

If `output=fifo`, these parameters take effect.

| parameter | description |
|-----------|-------------|
| path      | publish to the fifo at this path |
