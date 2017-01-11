---
post_title: DC/OS Ports
menu_order: 4
---

This topic lists the ports that are required to launch DC/OS. Additional ports may be required to launch the individual DC/OS services.

## All roles

### TCP

|Port   |DC/OS [component](/docs/1.8/overview/components/) and systemd unit   | 
|---|---|
|  61003 | REX-Ray (`dcos-rexray.service`) (default) |  
|  61053 |  Mesos DNS (`dcos-mesos-dns.service`) |
|  61420 | Erlang Port Mapping Daemon (`dcos-epmd.service`)  |
|61421 | Layer 4 Load Balancer (`dcos-minuteman.service`)  |  
|62053 |  DNS Dispatcher (`dcos-spartan.service`) |  
|62080 |  Virtual Networks (`dcos-navstar.service`)  |  
|62501 |  DNS Dispatcher (`dcos-spartan.service`)  |  
|62502 | Virtual Network Service (`dcos-navstar.service`)  |  
|62503 | Layer 4 Load Balancer (`dcos-minuteman.service`)  |  

### UDP

|Port   |DC/OS [component](/docs/1.8/overview/components/) and systemd unit   | 
|---|---|
| 61053 | Mesos DNS (`dcos-mesos-dns.service`) |
|  62053 |  DNS Dispatcher (`dcos-spartan.service`) |
|  64000 |  Virtual Networks (`dcos-navstar.service`) |

## Master

### TCP

|Port   |DC/OS [component](/docs/1.8/overview/components/) and systemd unit   | 
|---|---|
|  53 |  DNS Dispatcher (`dcos-spartan.service`) |  
|  80 |  Admin Router Service (`dcos-adminrouter.service`) |  
|  443 |  Admin Router Service (`dcos-adminrouter.service`) |  
|  1050 |  Diagnostics (`dcos-3dt.service`) |  
| 1801  |  OAuth (`dcos-oauth`) |  
|  2181, 2888, 3888 |  Exhibitor (`dcos-exhibitor.service`) |  
|  5050 |  Mesos Master (`dcos-mesos-master.service`) |  
|  7070 |  Package service (`dcos-cosmos.service`) |  
|  8080 |  Marathon (`dcos-marathon.service`) |  
|  8123 |  Mesos DNS (`dcos-mesos-dns.service`) |  
|  8181 |  Exhibitor (`dcos-exhibitor.service`) |  
|  9990 | Package service (`dcos-cosmos.service`) |  
|  15055 | History Service (`dcos-history-service.service`) |  
|  Dynamic | DC/OS Metronome (`dcos-metronome.service`) | 
|  Dynamic | System Package Manager API (`dcos-pkgpanda-api.service`) | 

### UDP

|Port   |DC/OS [component](/docs/1.8/overview/components/) and systemd unit   | 
|---|---|
|  53 |  DNS Dispatcher (`dcos-spartan.service`)  |

## Agent

### TCP

|Port   |DC/OS [component](/docs/1.8/overview/components/) and systemd unit   | 
|---|---|
|  5051 |  Mesos Agent (`dcos-mesos-slave.service`) |  
|  61001 |  Admin Router Agent (`dcos-adminrouter-agent`) |  
|  61002 | Diagnostics (`dcos-3dt.service`) |  
|  1025-2180 | Default advertised port ranges (for Marathon health checks) |  
|   2182-3887| Default advertised port ranges (for Marathon health checks) |  
|  3889-5049| Default advertised port ranges (for Marathon health checks) |  
| 5052-8079| Default advertised port ranges (for Marathon health checks) |  
|8082-8180| Default advertised port ranges (for Marathon health checks) |  
|8182-32000 | Default advertised port ranges (for Marathon health checks) |

  
