---
layout: post
title:  "CMDB IN 7 STEPS: EFFECTIVE CONFIGURATION MANAGEMENT"
categories: netdevops
tags: Network Engineer
date: 2022-8-3
---

# CMDB IN 7 STEPS: EFFECTIVE CONFIGURATION MANAGEMENT

Before we get into the nuts and bolts of implementing a CMDB in 7 steps but first, let’s make sure we’re all on the same page with respect to what a CMDB is. The CMDB (short for configuration management database) is a collection of data points called Configuration Items (CIs) that are under the control of [Change Management](https://www.beyond20.com/blog/youre-doing-it-wrong-change-management-edition/).

What the heck does that mean? Well, it means that any service asset (be it hardware, software, contract, SLA, what have you) that should only be modified under an approved request for change (RFC) belongs in your CMDB. And, not surprisingly, anything that is in your CMDB can only be modified under an approved RFC. This is an important distinction because you undoubtedly have many service assets which are not controlled by your Change Management process. Most organizations do not track things like free software, keyboards and mice, sometimes even monitors. This is primarily because 1) these items are generally very low cost, and 2) they are very hard to track. However, if you do happen to track these types of service assets, that’s OK, too. Just remember that they must also be maintained under Change Management. A CMDB *is not* just a repository of data points. It should also provide relationships between these data points to ensure smooth transitions within the larger ecosystem.

Okay! Now that we’ve got that sorted, let’s move on to how to build one:

### 1. Build a Logical Data Model of the Service Asset & Configuration Management Process with Relationships

The first step in the process is to create a logical model of the Service Asset and Configuration Management (SACM) process. The logical model defines the scope of SACM activities. In other words, the logical model explains how deeply the organization would like to trace SACM relationships. For example, at a basic level, it is possible to draw relationships between key services and the underlying infrastructure (hardware, servers, software, and interfaces). At the other extreme, SACM could trace relationships down to the workstation level. I recommend starting with a high-level approach by defining several key services and tracing them back to servers and interfaces.

![CMDB Logical Model](https://cdn2.hubspot.net/hubfs/2485558/CMDB%20Model.png)
If you have a logical model like the one in the example above, you know Server 2 is connected to redundant Switches A and B, which are in turn uplinked to Router XYZ, and that Server 2 is critical to the business as it relates to Service B20, then you have the type of risk analysis data that will help determine the likelihood that you can reboot Router XYZ at 2:00PM on a Thursday afternoon without causing an unplanned, major outage. This is why maintaining the CMDB is so critical. Without vigilant updates, it becomes an obsolete data repository that no one trusts, and it undermines your organization’s Change Management process.

### 2. Define Configuration Item (CI) Types

![CI Types](https://cdn2.hubspot.net/hubfs/2485558/CI%20Types.png)Next, you’ll define CI types based on the logical model developed in Step One. Essentially, this entails categorizing any and all configuration items you’re interested in managing (server, switch, router, database, etc.).

Warning: A lot of people go astray here by trying to do everything at once. Trying to map all the relationships and keep track of every asset you have is going to be extremely difficult, so start small. What are the most important things you need to keep track of? Where are your weak points? What is currently in scope for Change Management?

Our 2-day CMDB workshop will help your team create a logical CMDB model.

[GET STARTED](https://www.beyond20.com/cmdb-configuration-management-database-workshop)

### 3. Assign an Owner to Each CI Type

Next, you’ll identify and assign an owner for each CI type identified in Step Two. The CI Owner will be responsible for the remaining steps for their respective CIs.  For example, if Jane is the owner of all your servers, she will be responsible for deciding what information we need to know about each server, and how to gather that information.

![Server-Owner](https://cdn2.hubspot.net/hubfs/2485558/Server-Owner.png)

### 4. Define Attributes for Each CI Type

Now that you have owners defined for each of your CI Types, they will define attributes for their respective types. An Attribute, per the ITIL publications, is a piece of information about a CI such as name, location, version number, or cost. In our example above, Jane will define all the attributes we need know about our servers and record them in the CMDB.

### ![Define-attributes](https://cdn2.hubspot.net/hubfs/2485558/Define-attributes.png) 5. Identify Sources of Information for Your CIs (ie. Discovery)

Once CIs, attributes, and owners have been defined, it’s important to understand where that information can be found. For example, an auto discovery tool could be used to identify related components on the network. Other options include visually inspecting hardware components to obtain serial numbers, reviewing purchase and warranty documentation, and even reviewing invoices. 

### ![Define-Sources](https://cdn2.hubspot.net/hubfs/2485558/Define-Sources.png) 6. Map Relationships Between Assets 

This step provides context to the information collected in previous steps. Here, we’re mapping relationships between components. This is akin to taking the logical data model created in Step One and overlaying it with the actual hardware, software and infrastructure components that exist in your environment. When feasible, an auto discovery tool such as [ServiceNow](https://www.beyond20.com/servicenow-it-operations-management/) [IT](https://www.beyond20.com/servicenow-it-operations-management/) [Operations Management](https://www.beyond20.com/servicenow-it-operations-management/), [Cherwell Asset Management](https://www.beyond20.com/cherwell-asset-management/), or [OpsRamp](https://www.beyond20.com/partner/opsramp/) can dramatically reduce the amount of time it takes to perform this mapping. If using auto discovery is not possible, relationships can be drawn manually.

Word to the wise: Even though this *can* be done manually, keeping it up will be much more difficult! Just like with humans, relationships between CIs can be difficult to maintain! A tool will make the whole thing much more pleasant.

![Define-relationships](https://cdn2.hubspot.net/hubfs/2485558/Define-relationships.png)

### 7. Rollout One CI Type at a Time – Don’t Try to do it All at Once!

In this step, CI types and associated attributes are actually built or loaded in the CMDB. An incremental approach works best here. Build out one CI type first and verify that it is correct before moving on to the next. For example, if you start with Servers, identify *all of the servers*, then identify all the attributes and owners associated with each one. Your last step – an extremely important one – before moving on to the next CI type is to verify that the relationship mapping between servers and other components is correct. When that’s confirmed, onward.

![CMDB-rollout](https://cdn2.hubspot.net/hubfs/2485558/CMDB-rollout.png)

Creating a valuable, current CMDB is not something that will happen overnight. The steps outlined here take time to implement; how much time depends on the resources you have available in your organization. This takes effort. And it takes a lot of buy-in up and down the org chart. The good news is you probably already have a solid start, or maybe even a complete CMDB waiting to be maintained. Ultimately, active administration is the key to success when it comes to configuration management.
