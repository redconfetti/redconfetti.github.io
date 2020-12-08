---
layout: post
published: true
title: Amazon Web Services
description: An Introduction
image_url: /images/posts/2019-06-13-aws-cloud9.png
date: 2019-06-13 20:37:00 -0800
categories:
  - amazon web services
tags:
  - cloud9
  - aws
---

This article documents my exploration of [Amazon Web Services (AWS)].

## History

The first service publically offered by Amazon in 2004 was
[Amazing Simple Queue Service (SQS)],
a distributed message queueing service. Message queuing helps provide
communications between applications/services, with the concern or risk of
requests (messages) being lost or overburdening the systems processing the
messages.

Thereafter AWS was relaunched offering Amazon Elastic Compute Cloud (EC2) and
Amazon Simple Storage Service (S3).
<!--more-->

## Elastic Compute Cloud (EC2)

One of the most popular of the Amazon Web Services is
[Amazon Elastic Compute Cloud (EC2)]. I'm more familiar with dedicated or
colocated hosting, where an actual desktop or rack mount computer is setup for
you with some Linux variant installed so that you can SSH into it and configure
the server using your own chosen methods. I'm also familiar with using Virtual
Private Servers hosted by companies like [Linode] or [DigitalOcean].

Much like VPS hosting, EC2 instances are provided by [hypervisor] technology
running on physical machines, thus hosting multiple Virtual Machines (VMs).
EC2 uses the [Xen] hypervisor, whereas other VPS services may use the
open source [Kernel-based Virtual Machine (KVM)] module for Linux hosting,
[Hyper-V] for Windows hosting, or [VMware vSphere] for cross-platform
virtualization.

### VPS Comparison

So how does EC2 differ from typical VPS services?

#### Automation

A VPS provider may have some sort of API to automate the setup and configuration
of new virtual machines (VMs), but usually this has some limitations. EC2
is intended to be provided as a service that allows for management of EC2
instances through a manual interface or via web based APIs.

#### Hardware Abstraction

The hardware and networking details for EC2 instances are completely abstracted
for you. They're not necessarily confined to a single host machine like a VPS
is.

This can come with some challenges however when your needs require efficient
communication between nodes.

- [Amazon Virtual Private Cloud]

#### Resources

A VPS typically has fixed resources such RAM, bandwidth, disk space, and CPU.
EC2 allows you to modify these manually or via scripted automation.

EC2 also provides flexibility with [instance types] that vary in their
optimization of resources.

There are [general purpose] instances that range in power appropriate for micro
services or small web servers, to gaming servers or testing environments. There
are instances intended for [high performance computing]. There are instances
intended for [intense disk input/output (IO)] such as Big Data processing,
Database servers, or logging. There are instances intended for [optimized
memory usage] such as Memcached, Redis, or database servers. There are instances
optimized for [enhanced networking]. You can enable GPU acceleration to be used
with [Amazon Elastic Inference (EI)] for machine learning.

If you need high disk I/O capacity, which is typical for big data applications,
then you can use a service such as [Amazon Elastic Block Store (EBS)] with your
EC2 instances.

#### Billing

You can setup an EC2 instance for only a couple hours, days, or weeks, and only
be charged for the resources you used. A typical VPS service is billed monthly
or yearly, so short term use isn't economically viable.

## Dedicated Hosts

Some organizations require dedicated hosting for security compliance, or even
for software licensing.

## Container Services

[Amazon Elastic Container Service (ECS)] provides support to host applications
that have been configured to run in a [Docker] container. You can choose to
run your application from a network of EC2 instances, or use [Amazon Fargate] to
simplify the setup so that you can focus on developing your application.

AWS also supports the open-source container orchestration system known as
[Kubernetes] via the [Amazon Elastic Container Service for Kubernetes (EKS)].

Anyone that has used Docker knows that you need a container registry to store
your container images, so AWS also offers
[Amazon Elastic Container Registry (ECR)].

## Cloud Formation

You can automate the provisioning of your Cloud9 environment by using
[Amazon Cloudformation].

See [Automating AWS Cloud9](https://dzone.com/articles/automating-aws-cloud9)

## Cloud 9

[Amazon Cloud9]

Uses [Amazon Virtual Private Cloud (VPC)] to communicate with the EC2 instance.
A virtual private cloud (VPC) is a virtual network dedicated to your AWS account.
It is logically isolated from other virtual networks in the AWS Cloud.

## Types of Storage

[Instance Store vs Elastic Block Store (EBS)](https://aws.amazon.com/premiumsupport/knowledge-center/instance-store-vs-ebs/)

## Backup Methods

[How to Back Up Amazon EC2 Instances](https://www.cloudberrylab.com/resources/blog/backup-aws-ec2-instance/)

EC2 Backup Method 2: Creating a New AMI

EC2 instances reply on Elastic Block Store (EBS) for their file system. You can
configure Amazon to automatically create snapshots of your EBS file system
periodically (daily, weekly, etc).

Alternatively you can also create an [Amazon Machine Image (AMI)]

## Other Services

- [Amazon Lambda]
- [Amazon Elastic Load Balancing]
- [Amazon Simple Storage Service (S3)]
- [Amazon Elastic File System (EFS)]
- [Amazon Elastic Container Service (ECS)] - Container orchestration service
  that supports Docker containers
- [Amazon Relational Database Service (RDS)] - Provides database services such
  as Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle Database and SQL Server
- [Dockerizing a Ruby on Rails Application]

[amazon web services (aws)]: https://en.wikipedia.org/wiki/Amazon_Web_Services
[amazing simple queue service (sqs)]: https://en.wikipedia.org/wiki/Amazon_Simple_Queue_Service
[hypervisor]: https://en.wikipedia.org/wiki/Hypervisor
[xen hypervisor]: https://en.wikipedia.org/wiki/Xen
[kernel-based virtual machine (kvm)]: https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine
[hyper-v]: https://en.wikipedia.org/wiki/Hyper-V
[vmware vsphere]: https://en.wikipedia.org/wiki/VMware_vSphere
[cloud9]: https://aws.amazon.com/cloud9/
[linode]: https://www.linode.com/
[digitalocean]: https://www.digitalocean.com/
[amazon elastic compute cloud (ec2)]: https://aws.amazon.com/ec2/
[general purpose]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/general-purpose-instances.html
[instance types]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#AvailableInstanceTypes
[high performance computing]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/compute-optimized-instances.html
[intense disk input/output (io)]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/storage-optimized-instances.html
[optimized memory usage]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/memory-optimized-instances.html
[enhanced networking]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/enhanced-networking.html#supported_instances
[amazon elastic inference (ei)]: https://aws.amazon.com/machine-learning/elastic-inference/
[amazon virtual private cloud]: https://aws.amazon.com/vpc/
[amazon lambda]: https://aws.amazon.com/lambda/
[amazon elastic file system (efs)]: https://aws.amazon.com/efs/
[amazon simple storage service (s3)]: https://aws.amazon.com/s3/
[amazon elastic block store (ebs)]: https://aws.amazon.com/ebs/
[amazon elastic container service (ecs)]: https://aws.amazon.com/ecs/
[amazon relational database service (rds)]: https://aws.amazon.com/rds/
[amazon elastic load balancing]: https://aws.amazon.com/elasticloadbalancing/
[amazon cloudformation]: https://aws.amazon.com/cloudformation/
[amazon virtual private cloud (vpc)]: https://aws.amazon.com/vpc/
[dockerizing a ruby on rails application]: https://semaphoreci.com/community/tutorials/dockerizing-a-ruby-on-rails-application
[amazon fargate]: https://aws.amazon.com/fargate/
[amazon elastic container service for kubernetes (eks)]: https://aws.amazon.com/eks/
[kubernetes]: https://en.wikipedia.org/wiki/Kubernetes
[amazon elastic container registry (ecr)]: https://aws.amazon.com/ecr/
[docker]: https://aws.amazon.com/docker/
[amazon machine image (ami)]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instances-and-amis.html
