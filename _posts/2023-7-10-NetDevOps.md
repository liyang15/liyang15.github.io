---
layout: post
title:  "NetDevOps: A modern approach to AWS networking deployments"
categories: netdevops
tags: Network Engineer
date: 2023-7-10
---

# NetDevOps: A modern approach to AWS networking deployments

Networks have grown larger and more complex with time, but they continue to be the foundation upon which applications and services run. This critical component has demanding requirements to keep up with a high velocity application development world.

How can you enable your network to deliver these requirements with confidence? By adopting NetDevOps practices.

This post covers what NetDevOps is, its components (modularity, cultural changes, automation and infrastructure as code, and CI/CD), as well as how you can leverage it to deliver applications and services at high velocity. Lastly, it details a concrete use case applied to AWS cloud networking.

## Why NetDevOps?

It all started with DevOps – a combination of cultural philosophies, practices, and tools that increase an organization’s ability to deliver applications and services at high velocity. This means faster changes leveraging automation and rigorous testing, which shortens the overall software development lifecycle (SDLC).

But as a cloud/network engineer, why should you care? 

*“Every time we implement a network change, something goes wrong.”*

*– **Example Corp, Infrastructure Team Lead***

*“The network is too critical to upgrade.”*

*– **AnyCompany Manufacturing, Network Engineer***

*“Joe is on vacation and without him, we cannot make changes to the network.”*

*– **AnyCompany Oil & Gas, Cloud Engineer***

Many of the current network infrastructures aren’t managed using SDLC methods. Instead, they’re managed using more traditional techniques. This makes network changes manual and error-prone, as well as slow and sequential.

NetDevOps addresses these shortcomings by adopting DevOps in Networking. The end goal continues to be the high velocity delivery of applications and services. However, to meet that goal, networking infrastructure changes must be deployed quicker so they aren’t the bottleneck in the delivery cycle. The NetDevOps lifecycle stages are represented in the following figure.

NetDevOps orchestrates and automates network changes to reduce the network delivery lifecycle, treats the network as code to allow for version control, and reliably tests changes to make sure of quality and stability.

![NetDevOps stages consist of: Plan changes, create configuration templates, build specific configurations, test configurations, release configuration, deploy to production, operate the network, monitor the network.](https://d2908q01vomqb2.cloudfront.net/5b384ce32d8cdef02bc3a139d4cac0a22bb029e8/2022/06/18/Picture-1.png)

**Figure 1 – NetDevOps Stages**

## NetDevOps components

For a deeper understanding of NetDevOps, lets analyze the different components (practices, tools, and cultural philosophies) that holistically form its foundation.

### Modularity

You might be familiar with the term ‘monolith’, which is often used to describe an application with individual components that are tightly coupled with each other. In turn, a change in a single component affects the entire application. Because the impact area is big, it’s very sensitive to changes and requires holistic testing of the entire system. This creates complexity and leads to a slower pace of innovation. Most modern systems are moving away from monolithic and into modular architectures. This is done by breaking the monoliths into multiple smaller micro-services where we manage, maintain, and scale each component individually.

The same principles apply to the field of networking. You want to avoid creating monolithic network architectures where different networking components are tightly coupled with each other. Example 1 shows a monolithic network design transformation.

### Example 1 – A large VPC containing all applications and environments

As an organization, you probably have tens or hundreds of apps, and a version of each application is typically hosted in distinct environments, such as dev, test, or prod. As shown in the following figure, deploying all of your applications and environments in a single VPC is an example of a monolithic network design.

![Several applications running in different subnets in the same VPC.](https://d2908q01vomqb2.cloudfront.net/5b384ce32d8cdef02bc3a139d4cac0a22bb029e8/2022/06/18/Picture-2.png)

**Figure 2 – Multiple applications in a single VPC network architecture**

In this kind of architecture, a VPC level change like IP CIDR, DHCP, and DNS configuration or hybrid/internet connectivity will impact all of the applications. A better approach, highlighted in the following figure, would be to break down this monolith into a modular architecture, thereby isolating apps and environments into separate VPC’s. In this design, you can make network configuration changes to individual apps or environments without any impact on the rest of the setup.

![Several applications, each running in its own separate VPC.](https://d2908q01vomqb2.cloudfront.net/5b384ce32d8cdef02bc3a139d4cac0a22bb029e8/2022/06/18/Picture-3.png)

**Figure 3 – Single application per VPC network architecture**

Note that leveraging a single VPC is a great way to get started in AWS, but as you grow and deploy more apps, a modular architecture scales much better. Additionally, the segmentation strategy proposed is just an example. How you modularize your applications across multiple VPC’s must be tailored to your specific requirements. Refer to [Building a Scalable and Secure Multi-VPC AWS Network Infrastructure](https://docs.aws.amazon.com/whitepapers/latest/building-scalable-secure-multi-vpc-network-infrastructure/welcome.html) AWS whitepaper for guidance.

### Cultural changes

One of major differences between traditional networking practices and NetDevOps is corporate culture. As mentioned in the previous section, networks are historically monolithic architectures where multiple smaller components are tightly coupled together. In this model, changes in single components affect the entire architecture. This blast radius makes network changes sensitive and in need of heavy testing. Therefore, network operations and changes are often the responsibility of a single centralized team which evaluates the potential impact and maps out required actions.

A network’s tightly coupled nature and its perceived criticality foster a culture of manual, sequential operations where automation is infrequently used.

On the contrary, NetDevOps incentivizes a modular decoupled network architecture where changes can be automated without affecting every other component. This is a shift from the coupled, centralized, and manual model.

However, to make this model excel, teams should be empowered to operate independently and with complete ownership, instead of having to go through a centralized decision-making process.

Automated parallel network changes and operations enabled by rigorous testing mean that your delivery velocity will be higher.

### Automation and Infrastructure as Code

Network automation has existed for a long time, and we know its benefits. It streamlines manual tasks, reducing the chances of human errors and speeding up network deployments.

However, in a software defined networking (SDN) world, the network automation benefits are amplified, and they make an even bigger difference in the rates of speed and innovation for organizations.

Within AWS, you can define your network Infrastructure as Code (IaC). You templatize your network configurations, thus embedding your best practices and enterprise guardrails on them.

Afterward, you can make these templates available to individual application teams, and they can spin up new network segments at scale. Using predefined validated network configuration templates allows for the predictable and repeatable provisioning of networking components.

Furthermore, representing your network as code also lets you effectively track changes and maintain a history log. As your network configuration evolves, you create new versions of your templates. This is usually a challenge and can lead to ‘snowflake networks’ – networks with different networking configurations in similar components. IaC paired with source control lets you maintain a single source of truth and effectively track changes.

There are different IaC tools available that can help you reap these benefits, including: [AWS CloudFormation](https://aws.amazon.com/pt/cloudformation/) and [AWS Cloud Development Kit (AWS CDK)](https://aws.amazon.com/pt/cdk/).

In [AWS CloudFormation VPC template](https://docs.aws.amazon.com/codebuild/latest/userguide/cloudformation-vpc-template.html) official documentation, you can find an example of a CloudFormation template to create a VPC and its components.

### Continuous Integration/Continuous deployment (CI/CD)

IaC combined with modular architectures makes it easy for autonomous teams to define and deploy network infrastructure changes for what they’re trying to achieve without affecting the overall setup. That being said, most deployments still require configuration tracking, multiple testing levels, and manual approvals. This makes deploying changes in production environments time consuming. To accelerate this process, we can leverage CI/CD.

The idea is simple: you store your CloudFormation or AWS CDK templates (network configuration as code) in a version control system, such as or GitHub. When you commit a new version, as shown in the following figure, a pipeline gets triggered that automates several stages where you perform code validation, security checks, and network testing. Changes can be deployed to lower environments like test and staging, and if all goes well, pushed to your production environment. The best part is that everything is automated, including fallbacks in the case of any errors.

![Continuous Integration and Continuous Delivery pipeline stages: Source control, build, staging deployment, and production deployment](https://d2908q01vomqb2.cloudfront.net/5b384ce32d8cdef02bc3a139d4cac0a22bb029e8/2022/06/18/Picture-4.png)

**Figure 4 – NetDevOps CI/CD pipeline stages**

Let’s discuss the various stages of the pipeline in greater detail.

#### Build/Unit-Test Stage

In the NetDevOps, you don’t have a requirement to build or compile code. So, we can use this stage to run unit tests. Here you verify whether or not a piece of the codebase—the so-called “unit”—behaves as the developer intended. You can inspect the code to see if it’s secure, matches intent, and follows enterprise guardrails. If you author your code in CloudFormation, then you can leverage multiple open-source frameworks that let you build these validation tests. [CFN Lint](https://github.com/aws-cloudformation/cfn-lint) does linting, [CloudFormation guard](https://github.com/aws-cloudformation/cloudformation-guard) lets you define custom policies and validate your templates against those policies, and finally the [cfn-nag](https://github.com/stelligent/cfn_nag) tool looks for patterns in CloudFormation templates that may indicate insecure infrastructure.

Let’s look at some examples of validation checks that you can perform on your network infrastructure definitions.

- If you’re deploying firewall rules (e.g., Security Groups, ) you can check for overly permissive rules such as allowing public SSH access.
- For new VPC deployments –
  - You can make sure that VPC flow logs are enabled and internet access via public subnets is disabled by default.
  - You can check to make sure that application owners aren’t provisioning large VPC’s that will result in wasted IP space or overlapping CIDR’s.
- If you’re creating peering connections, then you can make sure that it doesn’t violate your security posture. For example, you might not want to peer your prod and non-prod environments.

Note that all of this validation is done before any network configuration is deployed. We recommend working backward from your enterprise guardrails and security requirements to come up with validations checks for a given CI/CD pipeline.

#### Integration Testing Stage

Once code validation is completed, deploy the network changes to a test or staging environment and run integration tests. Here you make sure that the changes you made work as intended with rest of the network. This can be done with simple ping tests or by using automated reasoning via tools like [VPC reachability analyzer](https://docs.aws.amazon.com/vpc/latest/reachability/what-is-reachability-analyzer.html) and AWS Network Manager [Route analyzer](https://docs.aws.amazon.com/vpc/latest/tgwnm/route-analyzer.html). For example, if you had a CI/CD pipeline to create a new VPC, you can either run ping tests from within the VPC to check for remote access (for example to on-premises or the internet), or use VPC reachability analyzer to check reachability to other VPCs or to the internet. Ideally you would want a combination of both options – VPC reachability analyzer and a ping test. Additionally, you can perform load and performance tests at this stage.

[Integrating Network Connectivity Testing with Infrastructure Deployment](https://aws.amazon.com/blogs/networking-and-content-delivery/integrating-network-connectivity-testing-with-infrastructure-deployment/) is a good reference post on network connectivity testing.

#### Deployment Stage

In this stage, you actually deploy the network configuration into your environment. This deployment can be to any of your environments, i.e., development, testing, production. Once you deploy the changes, you must check the health of your setup and rollback in case of any issues. Assuming that your infrastructure is defined as code, you can deploy your network as code templates as part of a deployment step using [AWS Code Pipeline](https://aws.amazon.com/pt/codepipeline/).

## Sample use case – Configuration changes

Now that you understand how NetDevOps can help you, let’s consider a scenario where you want to use a standardized method for provisioning new VPCs. As shown in the following figure, let’s assume that you already have an existing networking setup that consists of a single TGW and multiple VPC’s connected through it. For a new application being developed, you must add a new VPC to this pre-existing environment.

![Adding a new VPC to a pre-existing AWS Transit Gateway with other VPCs attached.](https://d2908q01vomqb2.cloudfront.net/5b384ce32d8cdef02bc3a139d4cac0a22bb029e8/2022/06/18/Picture-5.png)

**Figure 5 – Adding new VPC to pre-existing networking architecture**

To achieve this goal, you could use the and manually click through every needed action. However, if in a week you need to add another VPC, then you’ll have to do it again. With this approach, nothing will guarantee that enterprise best practices are adhered to and that you won’t make a slightly different configuration other than your memory.

In a NetDevOps approach, you would:

1. Create IaC templates for the networking resources with embedded best practices and enterprise guard rails.
2. Store the templates in a source control system.
3. Orchestrate the actions using a CI/CD pipeline leveraging unit and integration testing.

The high-level workflow for this sample use case is –

1. NetDevOps team will templatize VPC creation as code that will –
   1. Accept parameters from application teams, like application name, environment, CIDR ranges required, etc.
   2. Based on parameters from application team, create VPC and subnets and a TGW attachment request, as well as configure VPC route tables.
2. This template will be made available to application teams through a self-service portal, such as [AWS Service Catalogue](https://aws.amazon.com/servicecatalog/).
3. As shown in the following figure, a request from the application team on the self-service portal will trigger a deployment pipeline.
4. Unit tests for enterprise guardrails verification will be performed. Some example checks were shared in a previous section.
5. Upon successful checks validation, the pipeline will proceed to deploy the VPC and subnets, as well as initiate a TGW attachment request.
6. The Networking team can, by automation or manual intervention, approve the attachment request and associate it with a pre-defined route table.
7. Finally, you can run network connectivity testing leveraging ICMP pings and intent based configuration analysis using VPC reachability analyzer.
8. Successful testing concludes the workflow.

![The app team requests a VPC by providing specific inputs such as App name and environment. This request triggers a custom IaC template that creates and configures the VPC in the pre-existing environment.](https://d2908q01vomqb2.cloudfront.net/5b384ce32d8cdef02bc3a139d4cac0a22bb029e8/2022/06/18/Picture-6.png)

**Figure 6 – Creating a new VPC deployment pipeline – part 1**

## Conclusion

Leveraging NetDevOps lets you deliver applications and services at high velocity. This means faster changes and a reduced network delivery lifecycle.

In this post, you learned what NetDevOps is, why it’s needed in modern networking, and how it relates to DevOps practices. You also learned what components make up NetDevOps and how you can leverage those components to deliver value at high velocity. Furthermore, we highlighted that it’s not only about the technology, but also cultural changes affect your ability to deliver at high velocity, and they should be taken into consideration.

We have covered a use case from a high-level workflow perspective. If you’re curious about how it would look when implemented using [AWS CodeCommit](https://aws.amazon.com/codecommit/), [AWS CodePipeline](https://aws.amazon.com/pt/codepipeline/) and CloudFormation, then you can check out the [Using Infrastructure as Code to Manage Your AWS Networking Environment](https://aws.amazon.com/blogs/architecture/field-notes-using-infrastructure-as-code-to-manage-your-aws-networking-environment/) post.