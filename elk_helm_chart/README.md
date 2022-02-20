# Prerequisites
In this series we will focus on installing the Elastic stack , and will not go into the details of creating the Kubernetes cluster or installing Helm, so to follow along you should have good knowledge of Kubernetes and Helm, you should also have a working Kubernetes cluster with Helm installed , and kubectl command line tool ready and connected to your Kubernetes cluster.


Install Elastic Search
In this first article of the series, we will install a basic single node elastic search cluster with no security , in the next article we will see how to enable and configure security.

The installation process can be summarized in the following steps:

1. Configure storage for the elastic search cluster(AWS or Azure)
2. Add the elastic helm chart repository or local repository
3. Configure needed parameters for the elastic search helm chart (using values.yaml file)
4. Install the elastic search helm chart


# Configure needed parameters for the elastic search helm chart

We can now install the elastic search helm chart directly, but it will be installed with the default configuration values predefined in the chart , which is not very useful in our case.

To define our own configuration values , we will create a ‘values.yaml’ file , where we will override the configuration values we need to customize , then will use this file to install the chart. Helm will use the values we provided in this file to install the chart.


Note in our case, we’re using the default clusterName and nodeGroup, but when you want to run multiple Elasticsearch clusters in your Kubernetes cluster you should change the cluster name to something else. The masterService should be set to clusterName + nodeGroup of the Elasticsearch master cluster. For this article, we just deploy everything (master, data and ingest) on one cluster.

| Parameter  | Description | Value |
| ------------- | ------------- | ----------|
| clusterName  | This will be used as the Elasticsearch cluster.name | Will use the default value:elasticsearch      |
| nodeGroup  | This is the name that will be used for each group of nodes in the cluster  | Will use the default value:master|
|roles   |  Elasticsearch roles that will be applied to this nodeGroup |We will use a single node for all roles :master, ingest, data|
|replicas |Kubernetes replica count for the StatefulSet	|Will set it to 1 replica:1 |
|   esConfig  |Allows us to add any config files in /usr/share/elasticsearch/config/ | We will use this parameter to set elastic search configurations
|    image | The Elasticsearch Docker image| Will use the official image
|    imageTag |The Elasticsearch Docker image tag|Will use latest tag
|resources|Allows us to set the resources for the StatefulSet|You can set it according to your needs
|persistence|Enables a persistent volume for Elasticsearch data|Will enable it:enabled: true
|transportPort|The transport port that Kubernetes will use for the service|Will use the default:9300
|httpPort|The http port that Kubernetes will use for the healthchecks and the service|Will use the default:9200
