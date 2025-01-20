---
layout: article
title: "What is Amazon Redshift?"
date: 2023-01-26
modify_date: 2023-01-26
excerpt: "Is Amazon Redshift only a data warehouse product?"
tags: [AWS, Redshift]
mathjax: false
mathjax_autoNumber: false
key: aws-redshift-tips
---

***

## Amazon Redshift

Just a quick overview, what is Amazon Redshift ?

**Amazon redshift** is a **fully managed asset compliant cloud Datawarehouse service** provided by AWS that offers fast and predictable performance with seamless scalability. So ten of thousands of customers are using Redshift today.
{:.info}

**Amazon Redshift** is very optimize, in fact he has this **MPP architecture** i.e that's **Massive Paralell Precessing Architecture**. What you are getting at is, whith **Amazon Redshift** today, it support open files formats like **parquet, JSON, csv, ORC** and because of it **MPP** ypu get all the performance you need.

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214537112-3a6ec816-f578-4975-8a5e-e5f6d7c3d0c8.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>


Amazon Redshift is:

1. A **SQL compliant cloud datawarehouse**

2. He has **petabyte-scale capacity**, that means that Redshift datawarehouse, a single cluster can be pentabyte scale i.e it can store pentabyte of data inside this datawerouse solution. 
>So you can run complex queries and analytics and it work with data data visualization tools as well and others services(S3, DynamoDB, Kinesis, EMR)

> For customers, it's **SOC** compliance, **SOC 1**, **SOC 2**, **SOC 3**, **HIPAA**, **FedRAMP**  and lot mores, and give you an **SLA** of 99.9 % i.g if an entire cluster a single node goes down, it doesn't means that you will lose all your data because of how the architecture is and how it mirrors data to a secondary node.

3. **Amazon Redshift** is basically base on open-source **postgres** database but it's completely rewritten and very cost efficient compared to traditionnal on-premises datawarehouse because customers here are not doing any maintenance of infrastructure, management at all.

> To summarize, **Redshift** it has a **massively  parallel processing architecture** but to put it in a very simple terms, **one job is broken into small jobs and it's distributed accross different nodes
 to gives you result faster**.

 So if you have long running query and large processing jobs, it really speed performance.

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214536365-9b395b90-adbb-48d6-9a01-d55607bd9bc4.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>

* `Amazon Redshift` has a **columnar storage** so it's very good when it comes to run analytics you don't have this data written in the form of rows. 
* >> **Data from each column is stored together so the data can be accessed faster, without scanning and sorting all other columns** 
* **Each node has its own storage, its own compute and it's independent of other nodes in the cluster** so there are not going to be single point of failures what it also means is **you can easily add new nodes in the cluster or remove based on your required.** 

***

## Item 1: Amazon Redshift architecture

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214539972-bc2767d6-239a-41e9-9025-cdcf4804ee6b.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>

What we are looking at is a Redshift cluster and this Redshift cluster is made-up of **leader nodes** that is a **single leader node** and **multiple compute nodes** it could be a single compute node or more than one compute node. 

Your client applications(if they want to analyze and run analytical queries), could use a JDBC connectivity or ODBC connectivity to your Redshift cluster(leader node).So:

a. Client application connect to the leader node

b. Leader node gets all of those queries from the client applications 

c. Leader node **uses a mechanism to spread the queries across compute nodes**, get the results back aggregate those results and then give it back to the client 
> This is a the **basic overview of this architecture or Redshift architecture** 

### Item 1.1: Leader node

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214544314-8020a60f-a8b1-4aad-842b-7f5c02b4e179.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>

`Amazon redshift` cluster consists of 1 leader node and it could have multiple compute nodes. 
a. This leader node is automatically provisioned and customers do not pay for this, 

b.- it's **completely managed by AWS** and **automatically provisioned in every cluster**. Customers are not charged for the leader node and this leader node is basically your SQL entry point for your clients or for BI tools to access the cluster.

c. This leader node is basically your SQL entry point for your clients or for BI tools to access the cluster.

d. Your SQL clients they **don't have access to the computer once they will directly connect to the leader node.**

e. The idea of this leader node is that it will manage the communication between your client application and these compute nodes and it does the coordination of tasks between these different compute nodes as well.

f. This leader node is responsible for storing metadata, it is responsible for compiling queries and it is responsible to coordinate parallel processing across these compute nodes.

**For example:**  Let's say the client application sends a query to the leader node . Leader node will intelligently distribute and coordinate parallel running across compute nodes the SQL job,  so it takes that query and sends these queries to the compute nodes in the backend.

The leader node then will gather the results back from this compute nodes, aggregates them before returning the results back to the client. 

### Item 1.2: Compute node
What customers pay for? and what does all the work in the redshift cluster? **It is actually this compute nodes**, these are the SQL running powerhouses. 

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214580461-5f1460b7-2886-4d51-9065-afc0c5c9aa6b.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>


a. Compute nodes might even share data across each other to complete those queries, run those queries, take the results and immediately send the results back to the leader node when the computation is done.

b. They have a shared nothing architecture these compute nodes as you can see there could be one there could be a cluster of compute nodes and you can have 1 to 128 compute nodes depending on the type of nodes.

c. Compute nodes can even interact with S3 so compute nodes can load data to from S3 into the compute nodes for processing, stored data back to S3, backup data to S3.

d. These compute nodes have their own disks attached to them.

**For example:**  Let's take a look at one compute node. There are different types of compute nodes to choose, for example this compute node 1 is partitioned into slices so there are two slices right now that it's compute node is made-up of. Depending on the type of node there could be 2, 4, 8, 16, 32 slices as well depending on the type of node. Right now this node has two slices and the slices are the ones that are symmetric multiprocessing.

They basically have their own virtual cores they have their own memory they have their own local disk associated with them as well and these slices they operate in parallel and they only operate on the data that they own but they can request data from other slices if they have to use that to complete all of the processing.

#### What are the type of node? What redshift instance types you're looking at? 
<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214585529-6bd11048-7fd3-4870-aefc-5b35accdbc2c.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>

a. ``Dense storage or DS2`` are type of nodes that is deprecated now, it's not recommended to use this anymore but if customers have this it's still supported

b. What is recommended now is these two node types, the first of all is the most recommended one which is **RA3**, the second is **DC2**.

c. the first of all is **the most recommended one** is **RA3** which stands for **Redshift analytics 3** type of nodes and they have some features and maby capabilities. 

d. The second one is called **DC-2 or dense compute** and they have their own SSD storage so if you take a look at the **DC2** large has about 160 GB per node **DC28X** large has about 2.6 TB per node.  

e. With **Redshift analytics 3** you can scale the compute and storage independently because these **Redshift analytical 3 instances have their own managed S3 storage as well**, they can scale up to 64 TB,  so if you have a requirement wherein you might need one or two nodes to make up about 128 TB of storage, it is more economical for you to go in with **Redshift analytical 3** but if you're looking at lesser storage which is 160 or within this range or within 2.6 or 10 TB it might be more economical for you to go in with these type of storage (**DC3**) that has fixed local SSD storage.

***

## Item 2: Amazon Redshift Cluster resizing

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214609325-39d185ee-00fe-49cf-8cff-3345854d906c.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>

Remember how a customer may start off with two nodes but they might have a requirement of adding more nodes like another five or six nodes or there might be a requirement wherein now customers want to reduce the amount of nodes as well so initially there were two options which were provided **classic resize** and **elastic resize**.

**``Classic resize``** initially was used to do resizing for those older node types but going forward the recommended approach and method is actually this elastic resize. So going forward whether you want to add a new nodes remove new or remove nodes from the cluster any of resizing that you are doing **this first approach is the recommended approach from AWS** you should always use ``elastic resize`` 
> Adding or removing nodes to your clusters typically takes about a few minutes within 15 minutes it's achieved the second approach class security size is something that we don't recommend anymore because **elastic resize size can do everything that classic does as well**. So remember this important point as well 
 
## Item 3: Amazon Redshift Interfaces

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214613338-af658a03-56dd-4b4f-9e7f-595b405e5576.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>

``Amazon Redshift`` provides their built-in editor into the console as well so you can actually launch a cluster and there is an editor available for you to connect and run queries on top of your Redshift cluster. So we have the console to run some of these queries but you can also use ``CLI``, you can use ``SQL tools`` you can use ``Redshift  API`` itself these are commonly used by users to query your Redshift cluster. 

## Item 4: Amazon Redshift differentiating features

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214615162-62990f35-efc1-4c3d-9d37-01af1f7f5f70.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>

Let's take a look at some differentiating features of Redshift.

### Item 4.1: Fererated Query

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214615804-7305a6fd-752d-4969-b73d-80bd2690d828.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>

``Amazon Redshift Federated Query`` enables you to use the analytic power of ``Amazon Redshift`` to directly query data stored in ``Amazon Aurora PostgreSQL`` and ``Amazon RDS for PostgreSQL`` databases. For more information about setting up an environment where you can try out Federated Query, see [Accelerate Amazon Redshift Federated Query adoption with AWS CloudFormation](https://aws.amazon.com/blogs/big-data/accelerate-amazon-redshift-federated-query-adoption-with-aws-cloudformation/).

``Federated Query`` enables real-time data integration and simplified ETL processing. You can now connect live data sources directly in Amazon Redshift to provide real-time reporting and analysis. Previously, you needed to extract data from your PostgreSQL database to Amazon Simple Storage Service (Amazon S3) and load it to Amazon Redshift using ``COPY`` or query it from Amazon S3 with Amazon Redshift Spectrum. For more information about the benefits of Federated Query, see [Build a Simplified ETL and Live Data Query Solution using Amazon Redshift Federated Query](https://aws.amazon.com/blogs/big-data/build-a-simplified-etl-and-live-data-query-solution-using-redshift-federated-query/).


### Item 4.2: Lake House Architecture

<figure>
  <img src="https://user-images.githubusercontent.com/14333637/214645477-e0f81dd0-e077-440e-bd80-c44c35bd7af3.png" alt=".." title="Optional title" width="70%" height="70%"/>
	<figcaption></figcaption>
</figure>

As a modern data architecture, the Lake House approach is not just about integrating your data lake and your data warehouse, but it’s about connecting your data lake, your data warehouse, and all your other purpose-built services into a coherent whole. The data lake allows you to have a single place you can run analytics across most of your data while the purpose-built analytics services provide the speed you need for specific use cases like real-time dashboards and log analytics.

This Lake House approach consists of following key elements:

- Scalable Data Lakes
- Purpose-built Data Services
- Seamless Data Movement
- Unified Governance
- Performant and Cost-effective

## References

1. [Querying data with federated queries in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/federated-overview.html)
2. [Examples of using a federated query](https://docs.aws.amazon.com/redshift/latest/dg/federated_query_example.html)
3. [Best practices for Amazon Redshift Federated Query](https://aws.amazon.com/fr/blogs/big-data/amazon-redshift-federated-query-best-practices-and-performance-considerations/)
4. [Getting started querying data on remote data sources](https://docs.aws.amazon.com/redshift/latest/gsg/federated-query.html)
5. [https://docs.aws.amazon.com/redshift/latest/gsg/federated-query.html](https://aws.amazon.com/fr/blogs/big-data/announcing-amazon-redshift-federated-querying-to-amazon-aurora-mysql-and-amazon-rds-for-mysql/)
6. [Build a Lake House Architecture on AWS](https://aws.amazon.com/fr/blogs/big-data/build-a-lake-house-architecture-on-aws/)
