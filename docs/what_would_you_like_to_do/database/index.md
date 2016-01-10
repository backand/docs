## Introduction
Backand users typically fall into one of two categories. The first category works with the underlying data system as a series of objects, and doesn't spend a lot of time focused on the database underpinning the application. The second category, though, work with databases as a matter of course. Often they have existing databases that they want to build an interface on top of, with complex existing relationships that need to be modeled accurately by any framework put in front of it. Below we'll cover the database connectivity functionality offered by Backand, and how it can fit even the most complex of database needs.

## Bringing Your Own Database

Most back-end-as-a-service providers want you to work within their clearly-defined data roles. They often require you to port your data to their format, or provide only highly static interfaces to interact with and consume existing data. At Backand, we realize that the database is often the core of your business, and you need to be able to work with your data with minimal effort possible. That's why we have built a large suite of functionality around importing – and managing your existing SQL database. Backand can analyze your database structure and build a scaffold around your existing data quickly, allowing your application developers to very rapidly begin working with your existing database structure.

## Importing a Database

While you can create a new database from scratch using our JSON object modeling interface, the true power of Backand is shown when we integrate seamlessly with your existing database. When you connect your Backand application to an existing database on an external server, Backand quickly analyzes your database structure and constructs a meaningful interface on top of your data. This covers everything from basic table structure all the way up to complex foreign-key relationships and many-to-many mapping tables. Backand then uses this data to construct an API on top of your database, allowing you to interact with your data in a RESTful manner very quickly and easily.

## Staying In Sync

While an import process is beneficial, it quickly loses its ease if it does not accurately track changes. Many vendors expect your database schema to remain static, and want to work only with your data – requiring a re-sync whenever the database structure changes. With Backand, the need to re-synchronize is completely removed from the process. Backand automatically detects changes to your underlying database and updates its representation of your data, removing a needless tertiary step in the process to ensure that your application remains synchronized with your database's structure. Furthermore, data changes are synced back to the database server nearly instantaneously, ensuring that no matter how you fetch your data it will always be up-to-date.

## Summary

If you already have a database that you're looking to build into an application, your options for outsourcing the related back end can be limited. With Backand, however, you can bring your own database and import its structure directly. When you link your Backand application with your existing database you always have up-to-date access on both your database's schema and the data itself. By not having to perform awkward data transformations, or re-synchronize every time you make a change to the schema, Backand allows you to focus on the core of your application, keeping your database up-to-date no matter what.
