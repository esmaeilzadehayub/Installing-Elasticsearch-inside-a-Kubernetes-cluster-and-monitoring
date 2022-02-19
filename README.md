# Installing-Elasticsearch-inside-a-Kubernetes-cluster-and-monitoring
Installing Elasticsearch inside a Kubernetes cluster and monitoring

# What is Elasticsearch?
According to the Elasticsearch website:

Elasticsearch is a distributed, open source search and analytics engine for all types of data, including textual, numerical, geospatial, structured, and unstructured.

Elasticsearch is generally used as the underlying engine for platforms that perform complex text search, logging, or real-time advanced analytics operations. The ELK stack (Elasticsearch, Logstash, and Kibana) has also become the de facto standard when it comes to logging and it's visualization in container environments.

# Architecture
Before we move forward, let us take a look at the basic architecture of Elasticsearch:

![image](https://user-images.githubusercontent.com/28998255/154790975-bd3e6cb2-6845-42ac-b95c-e273047a5141.png)
![image](https://user-images.githubusercontent.com/28998255/154791466-31cae9cc-7399-4ca4-acf9-f85d3d9d6999.png)


The above is an overview of a basic Elasticsearch Cluster. As you can see, the cluster is divided into several nodes. A node is a server (physical or virtual) that stores some data and is a part of the elasticsearch cluster. A cluster, on the other hand, is a collection of several nodes that together form the cluster. Every node in turn can hold multiple shards from one or multiple indices. Different kinds of nodes available in Elasticsearch are Master-eligible node, Data node, Ingest node, and Machine learning node(Not availble in the OSS version). In this article, we will only be looking at the master and data nodes for the sake of simplicity.


# Master-eligible node
A node that has node.master flag set to true, which makes it eligible to be elected as the master node which controls the cluster. One of the master-eligible nodes is elected as the Master via the master election process. Following are few of the functions performed by the master node:

 1. Creating or deleting an index
 2. Tracking which nodes are part of the cluster
 3. Deciding which shards to allocate to which nodes

# Data node
A node that has node.data flag set to true. Data nodes hold the shards that contain the documents you have indexed. These nodes perform several operations that are IO, memory, and CPU extensive in nature. Some of the functions performed by data nodes are:

1. Data related operations like CRUD
2. Search
3. Aggregations


# Installation

We will be working with the list of contents :
Step1 — How to create a Namespace and Cluster Role?
Step2 — How to Create the Elasticsearch StatefulSet?
Step3 — How to create Kibana deployments and services?
Step4 — How to Create the Fluentd DaemonSet in the cluster?

