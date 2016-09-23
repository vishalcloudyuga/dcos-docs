---
post_title: Using Virtual IP Addresses
menu_order: 10
---
DC/OS can map traffic from a single Virtual IP (VIP) to multiple IP addresses and ports. DC/OS VIPs are name-based, which means clients connect with a service address instead of an IP address. DC/OS generates name-based VIPs such that the cannot collide with IP VIPs, so administrators don't need to carefully manage name-based VIPs to avoid collision. This also means that name-based VIPs can be automatically created when the service is installed.

A named VIP contains 3 components:

 * Private virtual IP address
 * Port (a port which the service is available on)
 * Service name

You can assign a VIP to your application from the DC/OS web interface. The values you enter when you deploy a new service are translated into the appropriate `portMapping` entry in your Marathon application definition. Toggle to `JSON mode` as you create your app to see and edit your application definition.

## Prerequisite:

*   A pool of VIP addresses that are unique to your application.

To create a VIP:

1.  From the DC/OS web interface, click on the **Services** tab and either click your service name or click "Deploy Service" to create a new service.

    *   Select the **Network** tab.
    *   To edit an existing application, click **Edit**. You can then select the **Network** menu option.

2.  Check the "Load Balance" checkbox, then fill in the LB Port, Name, and Protocol fields. As you fill in these fields, the service addresses that Marathon sets up will appear at the bottom of the screen. You can assign multiple VIPs to your app by clicking "+ Add an endpoint".

    The resulting JSON includes a `portDefinitions` field with the VIP you specified:
    
    ```
    "portDefinitions": [
        {
          "protocol": "tcp",
          "port": 5555,
          "labels": {
            "VIP_0": "/:5555"
          },
          "name": "my-vip"
        },
        {
          "protocol": "tcp",
          "port": 0
        }
      ]
    ```

    **Tip:** Toggle to **JSON Mode** to edit the JSON directly and to see the application definition you have created.

For more information on port configuration, see the [ports documentation][1].

## Using VIPs with DC/OS Services

Some DC/OS Services, such as Kafka, automatically create VIPs when you install them. The naming convention is: `broker.<service.name>.l4lb.thisdcos.directory:9092`.

To see the VIP for Kafka, run `dcos kafka connection` from the DC/OS CLI. The following is a sample response:

$ dcos kafka connection

```json
{
    "address": [
        "10.0.0.211:9843",
        "10.0.0.217:10056",
        "10.0.0.214:9689"
    ],
    "dns": [
        "broker-0.kafka.mesos:9843",
        "broker-1.kafka.mesos:10056",
        "broker-2.kafka.mesos:9689"
    ],
    "vip": "broker.kafka.l4lb.thisdcos.directory:9092",
    "zookeeper": "master.mesos:2181/dcos-service-kafka"
}
```

You can use this VIP to address any one of the Kafka brokers in the cluster.

 [1]: http://mesosphere.github.io/marathon/docs/ports.html
