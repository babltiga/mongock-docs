---
title: What is Mongock?
date: 2014-04-18 11:30:00 +0800 
permalink: /index.html
eleventyNavigation:
  root: true
  order: 0
---

<!--1. [Introduction](#introduction)
2. [Why Mongock?](#why-mongock%3F)
4. [How it works](#how-it-works)-->

<p class="text-center">
    <img src="/images/mongock-logo-with-title.jpg" width="65%" alt="Mongock">
</p>


## Introduction
Mongock is a Java based Migration tool as part of your Application code. It allows Developers to have ownership and control over Data migrations during the deployment process, allowing safer migrations in Distributed Systems as code and data changes are shipped together. 

It was originally designed for MongoDB Data migrations and the product is evolving to provide a wider suite of NoSQL / SQL Database compatibility and cover other feautures for transactional execution use cases that require state management in Distributed systems. 

------------------------------------------------

## Why Mongock?
Our mission is to enable Developers to deploy and manage stateful Data migrations in Distributed Systems in a safe and reliable manner. If you are looking for migrating data for your Java Application Services in a safe and reliable manner via Code, Mongock is your best bet.

In a nutshell:
- Mongock is a Java-based tool that you can import in your Application.
- Mongock promotes a Code-first approach for Migrations, allowing developers to write migration scripts in Java which ship with the Application code. This enables Code and Database changes to ship together.
- Mongock persists in your Database the changes made. This allows Developers to gain more control over the management of changes via Migrations, Rollbacks + other amazing features.
- It is the most reliable production-grade solution for MongoDB migrations currently in the market, compatible with Mongo Atlas and different MongoDB versions.
- Mongock is used in Distributed Systems as the solution implements a solid locking mechanism.
- Our team offers great support and is very responsive.
- We are an open source tool, operating under the Apache License 2.0 
- We maintain and update features regularly.
- Currently, Mongock is used by several tech companies in different industries.
- Adopted by well-known frameworks such as JHipster as part of the scaffolding.
- Can be used together with most, if not all, frameworks.
- Can run in Standalone projects without depending on frameworks.
- Great support with the Spring framework overall, with native implementations in SpringBoot.

Get more information about our support model at [dev@cloudyrock.io​](mailto:dev@cloudyrock.io​) and we can help you walking you to production. 

------------------------------------------------

## How it works

### 1. Scenario
Lets describe a scenario where the use of Mongock plays a big role:

We have a microservice called **client-service**, which uses the table/collection `clients`

As part of the development, we have a new User Story which requires your application to get some clients data from a third party system and persist them into database, in the `clients` table/collection. This is only required once and must be performed as part of the release.

In addition, the **client-service** is deployed in a Distributed environment running with more than one node and with horizontal scaling.

### 2. Your migration changes ([ChangeUnit](/migration/))
We first need to implement the migration to retrieve the data from the third party system and persist it in the database. For this we use the ChangeUnit, which is Mongock's unit of Migration. This will be where we write the code. 

<div class="successAlt">
<b>Note:</b> From version 5, ChangeLog annotation is deprecated (though remains for backwards compatibility). It's been replaced by <b>@ChangeUnit</b>.
</div>

Please, visit the [ChangeUnit section](/migration/) for more information.

### 3. The persistent layer ([Driver](/driver/))
We need to access the database for multiple reasons:
- To persist the client data we just retrieved.
- Mongock needs to track the ChangeUnits that have been executed, so the **client-service** doesn't execute them twice. 
-- As more than one instance of the client-service may be running simultaneusly in the environment, it will try to execute the same migration on startup. To prevent this, Mongock uses a pesimistic lock that is persisted in database.
-- In addition, this allows Mongock to store a history of changes and execute Rollbacks, amongst other operations.


For allowing Mongock to access the Database and persist state changes, we will use the **Mongock Driver**. The driver contains all the information and mechanisms to access to the database. Mongock provides multiple drivers for different databases. The developers can choose the most suitable and configure the driver by providing some information like the the target schema, etc.

Please, visit the [Driver section](/driver/) for more information.

### 4. Run the migration
Once we have implemented our migration in our changelog and configured the persistent layer using a Mongock driver, we need to run the migration. For this, we will use the **Mongock Runner**.

The Mongock Runner orchestrates the configurations, environment/framework settings, the driver and the ChangeUnits and runs all migrations in an orderly manner. The most common use case is that the  application won't start until the migration has been succesfully finalized (although this  behaviour can be altered). This means that if the list of migrations fail, the application will stop its startup process and exit. This will be retried again in the next startup and Mongock will continue the migration where it left.

Mongock provides multiple runners for framework compatibility: for standalone applications, Springboot, Micronaut(coming soon), etc. 

Please, visit the [Runner section](/runner/) for more information.

Great, we have the migration done, tested, deployed and working nicely. Awesome 🎉 

For a more in-depth description of Mongock, please visit the [Technical Overview](/technical-overview) section

### ... Cool, but we got a new user story! :scream:

But now a new User Story has arrived where it requires us to add a new field. Well, we already have Mongock setup, all it requires is to create another ChangeUnit to address this change in the same way we did previously. Don't worry, Mongock will take care of running only the ChangeUnits that haven't been executed yet.

<p class="noteAlt"> Note that a <b>ChangeUnit</b> represents a unit of migration, which means that each ChangeUnit is wrapped in a <b>transaction</b>. As soon as it's finished, it's commited.</p>


### ... other ways of running Mongock
The explained way of running Mongock is the common and traditional use case. However, Mongock offers more.

You can use the **Mongock CLI** to run the same migration, but also other operations like undo, list and more supported operations. The purpose is to provide a flexible manner of executing migrations.

Please, visit the [CLI section](/cli/) for more information.