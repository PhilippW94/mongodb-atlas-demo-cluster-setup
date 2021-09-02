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
3. Performance Advisor Demo Setup 
4. Atlas Search Demo Setup
5. Charts Demo Setup
6. GraphQL Demo Setup


The [Atlas Demo Discovery Playbook Cheatsheet](https://docs.google.com/document/d/1RZVWKsR6CjSKoByxyPiUxgaQ-opjNCSrYiRDCUwC23U/edit#heading=h.744mm6ty7947) provides a "manual" for a walk through the different Atlas Features and some relevant questions to ask. 


---
# Setup
## ![1](https://github.com/PhilippW94/Kafka_POV/blob/main/images/1b.png) Create Atlas Cluster
* Log-on to your [Atlas account](http://cloud.mongodb.com) 
* _(Optional)_ Locate the relevant Atlas Org (e.g. EMEA Demo Environment) and create a new Project, e.g. EMEA Demo. If this step has not been taken yet and no org with a preallocated Atlas credits system exists in your region, contact your Manager.
* Create an __M10__ based 3 node replica-set in a single AWS region of your choice with default settings. Make sure to select one of the following AWS regions:
  * Virginia (us-east-1)
  * Oregon (us-west-2)
  * Ireland (eu-west-1)
  * Sydney (ap-southeast-2)
  * Frankfurt (eu-central-1)
  * Mumbai (ap-south-1)
  * Singapore (ap-southeast-1)
* If none of those regions close:::: REALM!!!
* Give the cluster a meaningful name, e.g. **DemoCluster-DoNotDelete**.
* Once the Cluster is up and running you can proceed to the generation of a workload on the cluster

## ![2](https://github.com/PhilippW94/Kafka_POV/blob/main/images/2b.png) Generate Workload on Cluster
We will generate a workload on the cluster which makes use of the sample data set provided by MongoDB Atlas. The workload itself is originating from a Realm function triggered every minute. 
* In the cluster overview, click on the **...** button and select "Load Sample Dataset"
* Once the sample data set is loaded, navigate to Realm and create a new application. 
  * In **1 Name** give your application a name, e.g. "WorkloadGenerator"
  * In **2 Link your Database** select the previously created database cluster.
  * In **Advanced Configuration**, select Local and chose the AWS region you deployed your cluster in. 
