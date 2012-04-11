==========
Data Model
==========


The Usergrid data model relies on “entities” and “collections”. Usergrid entities are simply JSON objects that are the basic objects of your application. Usergrid provides a set of predefined entities, such as users, and new entity types can be created to contain application-specific content. Entities are grouped and stored internally in “collections”.


The Usergrid data model is roughly analogous to a database with rows in a table. However, Usergrid entities are flexible unlike conventional database records that have a rigid schema defining what can and can't be stored and how data is related. For this reason, Usergrid excels at managing data for social media applications since content is connected in the way that makes the most sense from a user’s perspective. Data retrieval is also simpler since it occurs at a higher logical level.




--------
Entities 
--------


Each Usergrid entity is defined at a minimum by a unique UUID and entity type, which is a string such as “user”, “group”, or “location”. Each application also has a similar UUID.
When you access the Usergrid system via the API, you typically provide the application
UUID and the UUID of the entity that you're modifying or retrieving. 


Here's a simple example of an entity::


 {
   "uuid" : "1c18ca40-90b2-11e0-b91b-12313f0204bb",
   "type" : "user",
   "created" : 1307415547108000,
   "modified" : 1307415547108000,
   "username" : "edanuff",
   "email" : "ed@anuff.com",
   "name" : "Ed Anuff"
 }


Although you can assign your own identification property to an entity instance, the entity UUID is the canonical and fastest way to access it.


-----------
Collections
-----------


.. image:: _static/application_entities.png


For every entity type, there is a root collection that contains all entities of that type. Root collections are automatically named using the plural form of the entity type. For example, an
entity of type "user" is stored in the "users" root collection, as shown in the diagram above.


Entities can also be members of collections held by other entities. For example, every group
maintains a collection of users. Entities can be in more than one collection but are always members of the root collection for their entity type.


Collections can be searched via multiple search iterations. Most predefined and all application-defined entity properties are indexed, allowing you to query collections more easily and more quickly. You can, for example, query all users in a group that share a biography property containing the word "California".




-------------------
Predefined Entities
-------------------


Usergrid predefines this set of entity types::


* user
* group
* role
* application
* activity
* device
* message
* asset
* folder
* event


Each entity type is stored in its own collection::


* /users
* /groups
* /roles
* /applications
* /activities
* /devices
* /activities
* /messages
* /assets
* /folders
* /events


You can also create any type of dynamic or customized entity. Dynamic entities are automatically stored in a collection with a name that is the plural form of the entity type. For example, creating an entity of type “cat” stores the entity in a collection named “cats”. As a consequence, you should choose entity types that are singular nouns.


-----------------
Entity Properties
-----------------


All entities have predefined properties although you can define any number of dynamic or customized properties. Predefined properties require specific data types for validation purposes, while dynamic properties can be any JSON data type. Usergrid relies on JSON representation because it is supported by almost all languages and can represent most types of data and content.




All entities have the following default properties:


=============  ===========  =========================================
Property       Type         Description
=============  ===========  =========================================
uuid           UUID         Entity unique id
type           string       entity type (e.g., "user")
created        long         UNIX timestamp of entity creation
modified       long         UNIX timestamp of entity modification
=============  ===========  =========================================


Dynamic entities also have an optional "name" property that is a string identifier.


Please refer to the :ref:`reference-section` for the schemas and business logic associated with Usergrid system-defined entities.


------------------
Entity Connections
------------------


One of the powerful features of Usergrid entities is that they can be connected
together in a completely dynamic fashion. This eliminates the typical
challenge of predefining object relationships in a conventional database. 


Any two entities can be connected together with a specific connection type. For
example, to emulate how Twitter follows users, you'd connect two user entities together with a connection type of “follows”. 


The Usergrid system organizes data for fast queries against connections. To find all other users that a specific user is following, Usergrid can perform a single “database read”. Of course, Usergrid’s built-in search technology manages the search for you. Since the system optimizes data storage for fast retrieval, generally you don’t need to worry about response time.  






--------------------------
Storage for Streaming Data 
--------------------------


Most modern applications struggle to manage streams of data whether that data is user-related content (such as comments, activities, and tweets) or system-generated content like event logs for analytics. Mobile applications in particular are prone to generating very large amounts of streaming data. In addition, streaming data must often be routed automatically to subscribers or filtered or counted.


Usergrid includes two entity types that make use of streaming data: activities and events.


Activities
----------


Activities are system-defined entities based on the activity streams specification at http://activitystrea.ms/. Activities are distributed automatically to followers and group members, so there is no need to construct publish/subscribe relationships manually as you would for a message queue.


Events
------


Events can be used for logging purposes and provide a simple way to retain data for business intelligence or analytics.