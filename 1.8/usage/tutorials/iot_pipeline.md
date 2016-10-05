---
post_title: Building an IoT Pipeline
menu_order: 0
---


In this tutorial you install and deploy a containerized Ruby on Rails app named Tweeter. Tweeter is an app similar to Twitter that you can use to post 140-character messages to the internet. Then, you use Zeppelin to perform real-time analytics on the data created by Tweeter.

Tweeter:

*   Stores tweets in the DC/OS [Cassandra][1] service.
*   Streams tweets to the DC/OS [Kafka][2] service in real-time.
*   Performs real-time analytics with the DC/OS [Spark][3] and [Zeppelin][4] services.

This tutorial demonstrates how you can build a complete IoT pipeline on DC/OS in about 15 minutes! You will learn:

*   How to install DC/OS services.
*   How to add apps to DC/OS Marathon.
*   How to route apps to the public node with the [Marathon load balancer][5].
*   How your apps are discovered.
*   How to scale your apps.

**Prerequisites:**

*   A DC/OS cluster with at least 5 [private agents][6] and 1 [public agent][6]. You can [deploy a cluster to the public cloud][7] or follow the [enterprise installation instructions][8].
*   The fully qualified domain name of your DC/OS [public agent][9].

# Install the DC/OS services you'll need

1.  From the DC/OS web interface **Universe** tab, install the following packages with a single click.
    
    **Tip:** You can also install DC/OS packages from the DC/OS CLI with the [`dcos package install`][11] command.
    
    *   **Cassandra** The Cassandra database is used on the backend to store the Tweeter app data. Cassandra will spin up to at least 3 nodes. You will see the Health status go from Idle to Unhealthy, and finally to Healthy as the nodes come online. This may take several minutes.
    
    *   **Kafka** The Kafka publish-subscribe message service receives tweets from Cassandra and routes them to Zeppelin for real-time analytics. Kafka will spin up 3 brokers.
    
    *   **marathon-lb** The [Marathon load balancer (marathon-lb)][12] is a supplementary service discovery tool that can work in conjunction with native Mesos-DNS.
    
    *   **Zeppelin:** Zeppelin is an interactive analytics notebook that works with DC/OS Spark on the backend to enable interactive analytics and visualization. Because it's possible for Spark and Zeppelin to consume all of your cluster resources, you must specify a maximum number of cores for the Zeppelin service. Choose the **Advanced Installation** option when you install Zeppelin. Then, click the **spark** tab and set `cores_max` to `8`. Click **Review and Install**.
        
        **Tip:** You can also do this from the DC/OS CLI:
        
        *   From the DC/OS CLI, create a JSON options file, here called `zeppelin-options.json`, that sets spark.cores.max to 8:
            
                {  
                   "spark":{  
                      "cores_max":"8"
                   }
                }
                
        
        Then, pass the file to `dcos package install` using the `--options` parameter:
        
               $ dcos package install --options=zeppelin-options.json zeppelin
            

2.  Verify that the Kafka and Cassandra services are healthy before moving on to the next step. You can check that they have a status of Healthy in the DC/OS web interface or use the following commands on the DC/OS CLI:
    
        $ dcos kafka connection
        ...
        $ dcos cassandra connection
        ...
        

**Note:** It can take up to 10 minutes for Cassandra to initialize with DC/OS because of race conditions.

# Deploy the containerized app

In this step you deploy the containerized Tweeter app.

1.  Go to the [Tweeter][13] GitHub repository and download the following files to your `dcos` directory:
    
    *   `tweeter.json`
    
    *   `post-tweets.json`
    
    *   `tweeter-analytics.json`

2.  Modify the Marathon app definition file `tweeter.json` with vi or another text editor of your choice. A Marathon app definition file specifies the required parameters for launching an app with Marathon.
    
        $ vi tweeter.json
        

3.  Edit the `HAPROXY_0_VHOST` label to match the hostname of your public agent node. Be sure to remove the leading `http://` and the trailing `/`. If you are using AWS, this is your public ELB hostname. It should look similar to this: `brenden-7-publicsl-1dnroe89snjkq-221614774.us-west-2.elb.amazonaws.com`.
    
           {
             "labels": {
               "HAPROXY_0_VHOST": "brenden-7-publicsl-1dnroe89snjkq-221614774.us-west-2.elb.amazonaws.com"
             }
           }
        

4.  Launch 3 instances of Tweeter with this command:
    
        $ dcos marathon app add tweeter.json
        
    
    **Tip:** The `instances` parameter in `tweeter.json` specifies the number of app instances. Use the following command to scale your app up or down:
    
        $ dcos marathon app update tweeter instances=<number_of_desired_instances>
        

5.  Verify that your app is added to Marathon by either finding it in the DC/OS web interface or running the following command from the DC/OS CLI:
    
        $ dcos marathon app list
        

The service talks to Cassandra via `node-0.cassandra.mesos:9042`, and Kafka via `broker-0.kafka.mesos:9557` in this example. Traffic is routed via the Marathon load balancer (marathon-lb) because you added the HAPROXY_0_VHOST tag on the `tweeter.json` definition.

Go to the Marathon web interface to verify your app is up and healthy. Then, navigate to `http://<public_agent_hostname>` to see the Tweeter UI and post a Tweet.

![Tweeter][14]

# Post 100K Tweets

Use the `post-tweets.json` app a large number of Shakespeare tweets from a file:

        $ dcos marathon app add post-tweets.json
    

The app will post more than 100k tweets one by one, so you'll see them coming in steadily when you refresh the page. Click the **Network** tab in the DC/OS web interface to see the load balancing in action.

The post-tweets app works by streaming to the VIP `1.1.1.1:30000`. This address is declared in the `cmd` parameter of the `post-tweets.json` app definition. The app uses the service discovery and load balancer service that is installed on every DC/OS node. You can see the Tweeter app defined with this VIP in the json definition under `VIP_0`.

# Add Streaming Analytics

Next, you'll perform real-time analytics on the stream of tweets coming in from Kafka.

1.  Navigate to Zeppelin at `https://<master_ip>/service/zeppelin/`, click **Import Note** and import tweeter-analytics.json. Zeppelin is preconfigured to execute Spark jobs on the DC/OS cluster, so there is no further configuration or setup required. Be sure to use `https://`, not `http://`.
    
    **Tip:** Your master IP address is the URL of the DC/OS web interface.

2.  Navigate to **Notebook** > **tweeter-analytics**.

3.  Run the Load Dependencies step to load the required libraries into Zeppelin.

4.  Run the Spark Streaming step, which reads the tweet stream from ZooKeeper and puts them into a temporary table that can be queried using SparkSQL.

5.  Run the Top Tweeters SQL query, which counts the number of tweets per user using the table created in the previous step. The table updates continuously as new tweets come in, so re-running the query will produce a different result every time.

![Top Tweeters][16]

 [1]: docs.mesosphere.com/1.8/usage/service-guides/cassandra/
 [2]: docs.mesosphere.com/1.8/usage/service-guides/kafka
 [3]: docs.mesosphere.com/1.8/usage/service-guides/spark/
 [4]: docs.mesosphere.com/1.8/usage/service-guides/zeppelin/
 [5]: https://github.com/mesosphere/marathon-lb
 [6]: /docs/1.8/overview/concepts/
 [7]: /docs/1.8/administration/installing/cloud/
 [8]: /docs/1.8/administration/installing/custom/
 [9]: /docs/1.8/overview/concepts/#public
 [10]: ../img/webui-universe-install.png
 [11]: /docs/1.8/usage/cli/command-reference/
 [12]: /docs/1.8/usage/service-discovery/marathon-lb/
 [13]: https://github.com/mesosphere/tweeter
 [14]: ../img/tweeter.png
 [15]: ../img/network-tab.png
 [16]: ../img/top-tweeters.png