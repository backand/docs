---
title: Backand Documentation

language_tabs:
  - shell: cURL
  - javascript

toc_footers:
  - <a href='https://www.backand.com'>Sign Up for a free app</a>
  - <a href='https://github.com/backand'>View our samples on GitHub</a>

includes:
  - backand_features
  - backand_dashboard
  - sdk
  - platform_specific
  - common_use_cases
  - integrations

search: true
---

# Documentation Overview

##What is Backand?
Backand's goal is to free up the front end development of web applications by providing a rich, robust, and scalable back end with minimal impact on the development process. The key feature offered by Backand is the [ORM](http://en.wikipedia.org/wiki/Object-relational_mapping), but most application back ends require more than just object management. Backand provides you with most of what your application needs automatically, offering features like tracking data changes, logging, role-based security, back-office connectivity, and much more. You can even create your own server side actions with JavaScript, running custom server side queries and easily integrating with third party services.

###Backand ORM
Backand [ORM](http://en.wikipedia.org/wiki/Object-relational_mapping) automatically provides you with a REST API to perform [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations against your database. You can connect an existing database you already have, or create a new database with the Backand dashboard. When you create a database at Backand it is automatically populated in Amazon's AWS Relational Database Service (AWS RDS), providing an isolated database server that you can scale automatically, or even use in other applications entirely independent of Backand. Even though Backand focuses on software services as opposed to platform services, when you register a database with Backand it is truly your database.

###SQL and NoSQL - The Best of Both Worlds
Through the flexibility of Backand's API, you have the ability to work with your data at whatever level you desire. You can perform basic [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations in a couple ways. In one way you can mimic a SQL database by simply returning a shallow representation of the object, with foreign key references remaining as simple IDs in the response data. However, you can also perform deep queries that resolve all of the underlying data objects into a single set of response data, giving you the level of object detail that you often see with NoSQL databases. With a simple parameter change, you can switch between the two patterns at will!

### Using cURL
```shell
# Retrieve all Lists in your Organization
# This call stores the MASTER_KEY and USER_KEY into environment variables
# Following this pattern will enhance the security and reliability of any console
# calls made to the API
curl https://api.backand.com/1/objects/items -u $MASTER_KEY:$USER_KEY

# You can perform the same task using URL parameters:
curl https://api.backand.com/1/objects/items?authorization=basic+$MASTER_KEY:$USER_KEY
```
