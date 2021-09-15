# MongoDB Atlas Demo Cluster Setup Manual

__How to set up a Demo Cluster in MongoDB Atlas including showcases of Realm, Charts and Atlas Search__

__SA Maintainer__: Philipp Waffler<br/>
__Time to setup__: 120 min <br/>
__Time to execute__: 60 min <br/>


---
## Description Contents
There are multiple steps involved in setting up a fully fledged demo cluster. In case you have further ideas about features of MongoDB Atlas that are easy to demo, let me know!

The following steps need to be stepped through in order to set up the Demo Cluster fully:

1. [Create Atlas Cluster](#-create-atlas-cluster)
2. [Generate Workload on Cluster](#-generate-workload-on-cluster)
3. [Performance Advisor Demo Setup](#-performance-advisor-demo-setup)
4. [Atlas Search Demo Setup](#-atlas-search-demo-setup)
5. [Charts Demo Setup](#-charts-demo-setup)
6. [GraphQL Demo Setup](#-graphql-demo-setup)
7. [Final Step - Permissions Setup](#-permissions-setup)


The [Atlas Demo Discovery Playbook Cheatsheet](https://docs.google.com/document/d/1RZVWKsR6CjSKoByxyPiUxgaQ-opjNCSrYiRDCUwC23U/edit#heading=h.744mm6ty7947) provides a "manual" for a walk through the different Atlas Features and some relevant questions to ask. 


---
# Setup
## ![1](https://github.com/PhilippW94/Kafka_POV/blob/main/images/1b.png) Create Atlas Cluster
* Log-on to your [Atlas account](http://cloud.mongodb.com) 
* _(Optional)_ Locate the relevant Atlas Org (e.g. EMEA Demo Environment) and create a new Project, e.g. EMEA Demo. If this step has not been taken yet and no org with a preallocated Atlas credits system exists in your region, contact your Manager.
* Create an __M10__ based 3 node replica-set in a single AWS region of your choice with default settings. If feasible, make sure to select one of the following AWS regions:
  * Virginia (us-east-1)
  * Oregon (us-west-2)
  * Ireland (eu-west-1)
  * Sydney (ap-southeast-2)
  * Frankfurt (eu-central-1)
  * Mumbai (ap-south-1)
  * Singapore (ap-southeast-1)
* Give the cluster a meaningful name, e.g. **DemoCluster-DoNotDelete**.
* Once the Cluster is up and running you can proceed to the generation of a workload on the cluster

## ![2](https://github.com/PhilippW94/Kafka_POV/blob/main/images/2b.png) Generate Workload on Cluster
_Uses robbertkauffmans script for [slow running queries](https://github.com/robbertkauffman/mdb-slow-running-queries)._ This script can also be found in this repo in _RealmFunction.js_.

We will generate a workload on the cluster which makes use of the sample data set provided by MongoDB Atlas. The workload itself is originating from a Realm function triggered every minute. 
* In the cluster overview, click on the **...** button and select "Load Sample Dataset"
* Once the sample data set is loaded, navigate to Realm and create a new application. 
  * In **1 Name** give your application a name, e.g. "WorkloadGenerator"
  * In **2 Link your Database** select the previously created database cluster.
  * In **Advanced Configuration**, select Local and choose the AWS region you deployed your cluster in. _(If this region is not available in the dropdown, choose the geographically closest one)_ 
* Once the app is created, navigate to Triggers. Click on **Add a Trigger** and fill out the fields as shown below. <img src="https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/d300b45fb0f4f93e071b13952af4942cf1de52c6/media/Screenshot%202021-09-02%20at%2021.12.37.png?raw=true" width="600">
  * Select **Trigger Type** as _Scheduled_
  * Choose a **Name**, e.g. _workloadGenerator_
  * Ensure **Enabled** is set to _On_
  * **Scheduled Type** is set to _Basic_. **Repeat once by** set to _every minute_
  * Under Function, **Select An Event Type** is set to _Function_
  * Select _+ New Function_
  * Choose a **Function Name**, e.g. _workloadGeneratorFunc_
  * In the Function code, paste the contents of the [RealmFunction.js file](https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/d300b45fb0f4f93e071b13952af4942cf1de52c6/RealmFunction.js)
* Click **Save** and subsequently **Review & Deploy Changes**
* Check if everything is working by checking the Realm Logs. After a while, the Logs [should look like this](https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/main/media/Screenshot%202021-09-02%20at%2021.21.57.png?raw=true).

The final result after a while should look as follows:
* [Cluster Overview](https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/main/media/Screenshot%202021-09-02%20at%2021.42.27.png)
* [Monitoring](https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/main/media/Screenshot%202021-09-02%20at%2021.43.07.png)
* [Profiler](https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/main/media/Screenshot%202021-09-02%20at%2021.44.34.png)

## ![3](https://github.com/PhilippW94/Kafka_POV/blob/main/images/3b.png) Performance Advisor Demo Setup
Atlas will recommend with the above setup improvements for the [Create Indexes](https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/main/media/Screenshot%202021-09-02%20at%2021.33.22.png?raw=true), [Drop Indexes](https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/main/media/Screenshot%202021-09-02%20at%2021.33.27.png) and [Improve Schemas](https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/main/media/Screenshot%202021-09-02%20at%2021.34.06.png?raw=true) category by itself.

**WARNING**: Until the recommendations appear, some time will pass. Typically **Improve Schemas** will be showing recommendations after about two hours, **Create Indexes** after about 1-2 days and **Drop Indexes** after 1-2 weeks. 

## ![4](https://github.com/PhilippW94/Kafka_POV/blob/main/images/4b.png) Atlas Search Demo Setup
For the setup of the Atlas Search Demo, follow the steps described [Keynote from 2019](https://docs.google.com/presentation/d/14kSQnz98EOCORveEE1PYfxa5JMV32Z9LRxOSP9fFppA/edit#slide=id.p1).

## ![5](https://github.com/PhilippW94/Kafka_POV/blob/main/images/5b.png) Charts Demo Setup
For the setup of the Charts Demo, follow the steps described in the Charts Docs. There are two dashboards to be created, [Order Data](https://docs.mongodb.com/charts/saas/tutorial/order-data/order-data-tutorial-overview/) and [Movie Data](https://docs.mongodb.com/charts/saas/tutorial/movie-details/movie-details-tutorial-overview/)

## ![6](https://github.com/PhilippW94/Kafka_POV/blob/main/images/6b.png) GraphQL Demo Setup
For the setup of the GraphQL Demo, follow the steps described in the POVs as part of [SA Demotoolkit](https://github.com/10gen/pov-proof-exercises/tree/master/proofs/47).

## ![7](https://github.com/PhilippW94/Kafka_POV/blob/main/images/7b.png) Permissions Setup
In order to avoid accidents and mishaps that might result in your hard work being deleted, you should set up adequate permissions for the users of your demo cluster. 

Make use of the _Team_ feature on the project access level. A dedicated _Admin Team_ with the _Project Owner_ role and an _Everyone_-Team with the _Project Data Access Admin_ and _Project Cluster Manager_ role will support the desired usage pattern. Members of the _Everyone_-Team will be able to create a new cluster and edit configurations of existing ones. At the same time, they will not be able to terminate an existing instance - which could possibly be the thoroughly set up instance by you. The following shows an example of how the _Team_ configuration could look like:

<img src="https://github.com/PhilippW94/mongodb-atlas-demo-cluster-setup/blob/main/media/Screenshot%202021-09-15%20at%2013.42.42.png?raw=true" width="600"> 
