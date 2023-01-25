# Managing-TLS-secure-Kafka-on-Kubernetes-using-KubeDB

Good afternoon everyone,

Today I would like to talk to you about Kafka, one of the most popular open-source distributed streaming platforms. Kafka is used for building real-time data pipelines and streaming apps. It is a highly scalable, fault-tolerant, and high-throughput platform that enables the collection, storage, and processing of streaming data.

Kafka is built on a distributed architecture, where data is stored in a fault-tolerant and distributed manner across a cluster of machines. This allows for horizontal scalability, high availability, and high throughput. One of the key features of Kafka is its ability to handle high-velocity, high-volume, and high-variety data streams.

In this talk, I would like to focus on Kafka in Kraft mode, which is a new way to run kafka on kubernetes. Kraft mode eliminates the need for a separate Zookeeper cluster, which is traditionally used for maintaining the configuration and state of the kafka cluster. Instead, it uses the Kubernetes API to manage the configuration and state of the kafka cluster.

Kraft mode provides several benefits over traditional kafka deployments. For example, it eliminates the need for a separate Zookeeper cluster, which reduces the operational complexity and cost of running kafka. Additionally, kraft mode utilizes the Kubernetes API to manage the configuration and state of the kafka cluster, which simplifies the scaling and management of kafka clusters.

One of the main benefits of Zookeeperless kafka is it's simplicity. Zookeeper is a centralized service that is used for maintaining configuration and state in a kafka cluster. It is responsible for maintaining the metadata about the kafka topics and partitions, and it also coordinates the leader election for partitions. However, Zookeeper is a separate service that needs to be deployed, configured, and managed, which adds complexity to a kafka deployment.

In Kraft mode, the Kubernetes API is used to manage the configuration and state of the kafka cluster, which eliminates the need for a separate Zookeeper service. This simplifies the deployment, scaling, and management of kafka clusters. Additionally, since kraft mode utilizes the Kubernetes API, it can leverage the built-in Kubernetes features such as automatic scaling, self-healing, and automatic rolling updates.

Another important aspect of Kraft mode is its ability to run kafka on kubernetes. Kubernetes is a powerful and flexible platform for container orchestration. It provides built-in features such as automatic scaling, self-healing, and automatic rolling updates. When running kafka on kubernetes, these features can be leveraged to provide a highly available, scalable, and fault-tolerant kafka cluster.

Kraft mode also provides several other benefits, such as the ability to leverage Kubernetes features such as automatic scaling, self-healing, and automatic rolling updates, as well as the ability to run kafka on kubernetes. Additionally, Kraft mode is an open-source project, which means that it is free to use and can be easily customized to meet the specific needs of your organization.


Historically, the Kafka control plane was managed through an external consensus service called ZooKeeper. One broker is designated as the controller. The controller is responsible for communicating with ZooKeeper and the other brokers in the cluster. The metadata for the cluster is persisted in ZooKeeper.





Going forward, the Kafka control plane will be based on a new internal feature called KRaft. The dependency on ZooKeeper will be eliminated. In KRaft, a subset of brokers are designated as controllers, and these controllers provide the consensus services that used to be provided by ZooKeeper. All cluster metadata will be stored in Kafka topics and managed internally.

At the time of this writing, KRaft is still in preview, but it will soon be the default, so we will focus on it for the rest of this control plane module.

There are many advantages to the new KRaft mode, but we’ll discuss a few of them here.

Simpler deployment and administration – By having only a single application to install and manage, Kafka now has a much smaller operational footprint. This also makes it easier to take advantage of Kafka in smaller devices at the edge.
Improved scalability – As shown in the diagram, recovery time is an order of magnitude faster with KRaft than with ZooKeeper. This allows us to efficiently scale to millions of partitions in a single cluster. With ZooKeeper the effective limit was in the tens of thousands.
More efficient metadata propagation – Log-based, event-driven metadata propagation results in improved performance for many of Kafka’s core functions.

In KRaft mode, a Kafka cluster can run in dedicated or shared mode. In dedicated mode, some nodes will have their process.roles configuration set to controller, and the rest of the nodes will have it set to broker. For shared mode, some nodes will have process.roles set to controller, broker and those nodes will do double duty. Which way to go will depend on the size of your cluster.

The brokers that serve as controllers, in a KRaft mode cluster, are listed in the controller.quorum.voters configuration property that is set on each broker. This allows all of the brokers to communicate with the controllers. One of these controller brokers will be the active controller and it will handle communicating changes to metadata with the other brokers.

All of the controller brokers maintain an in-memory metadata cache that is kept up to date, so that any controller can take over as the active controller if needed. This is one of the features of KRaft that make it so much more efficient than the ZooKeeper-based control plane.

