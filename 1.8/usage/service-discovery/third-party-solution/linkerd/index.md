---
post_title: linkerd
menu_order: 0
---

[linkerd][1] is a service mesh for DC/OS. It is installed on every agent, and acts as a transparent, load-balancing proxy, providing service discovery and resilient communication between services.

The linkerd Universe package is configured to use Marathon for service discovery. This means that applications and services can refer to each other by their Marathon task name. For example, a connection to `http://myservice` made through linkerd will be load-balanced over instances of the Marathon application `myservice`, without using DNS. linkerd can also be configured to use dedicated service discovery systems such as ZooKeeper, Consul or etcd (as well as DNS itself) and to failover between these systems and/or Marathon.

![diagram](img/diagram.png)

In addition to service discovery and resilient communication, linkerd provides a uniform layer of visibility across all services and a convenient service metrics dashboards.

## Next Steps

- Install linkerd and linkerd-viz by following the [linkerd Tutorial][2].
- For a step-by-step example with a sample app, read the blog post: [DC/OS Blog: Service Discovery and Visibility with Ease on DC/OS][3]

 [1]: https://linkerd.io
 [2]: /docs/1.8/usage/tutorials/linkerd/
 [3]: https://dcos.io/blog/2016/service-discovery-and-visibility-with-ease-on-dc-os/index.html
