# Configuring provenance capture

One of the strengths of CamFlow is the ability to fine-tune the provenance information it captures.

The configuration file is `/etc/camflow.ini`.

To apply a new configuration, ???.

## Sample configuration

Here is a sample `/etc/camflow.ini` configuration.

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
;opaque=vagrant
;track=vagrant
;propagate=vagrant

[group]
;opaque=vagrant
;track=vagrant
;propagate=vagrant

[secctx]
;track=system_u:object_r:bin_t:s0
;propagate=system_u:object_r:bin_t:s0
;opaque=system_u:object_r:bin_t:s0
```

## Configuration parameters

Following is a list of the parameters and their effects, broken down by section.
A "boolean" parameter accepts values "true" or "false".

### provenance

| parameter | description |
|-----------|-------------|
| `machine_id`  | unique identifier for the machine in provenance records, use hostid if set to 0 |
| `enabled`     | boolean; enable provenance capture? |
| `all`         | boolean; record provenance of all kernel objects? |
| `node_filter` | do not capture this kind of node (i.e. vertex) |
| `relation_filter`           | do not capture this kind of relation (i.e. edge) |
| `propagate_node_filter`     | ??? |
| `propagate_relation_filter` | ??? |

#### node_filter

You can specify the node_filter parameter multiple times, with a different node type each time.
See [here](https://github.com/CamFlow/camflow-dev/blob/master/docs/VERTICES.md) for the list of supported node types.

#### relation_filter

You can specify the relation_filter parameter multiple times, with a different relation type each time.
See [here](https://github.com/CamFlow/camflow-dev/blob/master/docs/RELATIONS.md) for a complete list of edge types.

#### propagate_node_filter

As with node_filter, you can specify this parameter multiple times for various node types.

#### propagate_relation_filter

As with relation_filter, you can specify this parameter multiple times for various relation types.

### compression

TODO I couldn't find any relevant references to "compression" in the SoCC paper describing CamFlow.
As a result, this description is based on the concept from Ma 2018 "Kernel-Supported Cost-Effective Audit Logging for Causality Tracking". 
Let's see if I'm right...maybe only for the "duplicate" parameter?

"Compressing" provenance means emitting as few provenance records as possible to capture an interaction.
For example, if a process reads a file three times, then a compressed provenance record would contain only one read relation while a complete provenance record would contain three relations.

This is desirable if the goal of provenance collection is to build a provenance graph.
However, if you are trying to perform a security audit, then the fact and timing of multiple accesses may be of interest.
In this example compression may be undesirable.

| parameter | description |
|-----------|-------------|
| `node`      | boolean; compress nodes? |
| `edge`      | boolean; compress edges? |
| `duplicate` | boolean; ??? |

### file

This describes provenance capture behavior for files.

| parameter | description |
|-----------|-------------|
| `opaque`    | provenance is not captured for any interactions with this file |
| `track`     | directly track any information flow to/from this file and any process resulting from the execution of programs built from it |
| `propagate` | transitively track any information flow to/from this file |

#### track

Use `track` if you want the provenance information to include every time this file is read or written.
TODO: What does "built from it" mean?

#### propagate

Use `propagate` if you want the provenance information to track the flow of data out of this file, through other processes, into other files, etc.

### ipv4-egress

Track information leaving the system being monitored.

| parameter | description |
|-----------|-------------|
| `propagate` | similar to file, but for data to this IPv4 address |
| `record`    | ??? |

Specify an IPv4 address using the format ???.

### ipv4-ingress 

Track information entering the system being monitored.

| parameter | description |
|-----------|-------------|
| `propagate` | see ipv4-egress |
| `record`    | see to ipv4-egress |

### user

Like `file`, but for users.

| parameter | description |
|-----------|-------------|
| `opaque`  | similar to file, but for this username |
| `track`   | " |
| `propagate` | " |

TODO Can I also specify uid?

### group

Like `file`, but for groups.

TODO Can I also specify gid?

### secctx

Same as `file`, but for security contexts.

Specify a security context using the format ???.

## Additional information

Further information about these parameters may be gleaned by perusing the [header files](https://github.com/CamFlow/camflow-dev/tree/master/security/provenance/include) in the kernel module.

# Configuring provenance publication

After CamFlow captures provenance, you can configure *where* and *in what format* the provenance information is published.

The configuration file is `/etc/camflowd.ini`.

To apply a new configuration, run `systemctl restart camflowd.service`.

## Sample configuration

Here is a sample `/etc/camflowd.ini` configuration.

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
| `output`  | where to publish provenance records? |
| `format`  | in what format to publish provenance records? |

#### output

| value   | effect |
|---------|--------|
| `null`  | discard provenance records |
| `mqtt`  | publish to an MQTT service |
| `unix_socket` | publish to a UNIX socket |
| `fifo`  | publish to a named pipe ("FIFO") |
| `log`   | publish to a log file |

#### format

| value | effect |
|-------|--------|
| `w3c`        | use [w3c PROV-JSON](./w3c.md) format |
| `spade_json` | use [SPADE JSON](https://github.com/ashish-gehani/SPADE/wiki/Reporting-provenance-using-JSON) format |

### log

If `output=log`, these parameters take effect.

| parameter | description |
|-----------|-------------|
| `path`      | publish to this file |

### mqtt

If `output=mqtt`, these parameters take effect.

| parameter   | description |
|-------------|-------------|
| `address`   | MQTT broker |
| `username`  | credentials |
| `password`  | credentials |
| `qos`       | delivery guarantees |

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
| `address`   | publish to the socket at this path |

### fifo

If `output=fifo`, these parameters take effect.

| parameter | description |
|-----------|-------------|
| path      | publish to the fifo at this path |
