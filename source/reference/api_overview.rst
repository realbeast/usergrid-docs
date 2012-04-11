.. _api-operations:


=======================
Usergrid API Operations
=======================


Usergrid uses a pure REST API. (See `Representational State Transfer
<http://en.wikipedia.org/wiki/Representational_State_Transfer>`_ at Wikipedia
for more information about the principles behind this type of API.) 


A REST API is built as a collection of resources. Resource locations are described by paths that are related intrinsically to collections and entities in collections. When building a REST API, the challenge is to represent the data and action upon the data as a path to a resource that can be created, retrieved, updated, or deleted. The HTTP methods POST, GET, PUT, and DELETE correspond to the actions that are applied to resources.


In forming Usergrid API requests, resource paths are specified as URLs. For example, to cause the application "myapp" to retrieve data about a user named "edanuff", you’d construct the following API request::


  http://api.usergrid.com/myapp/users/edanuff


To get a listing of everything the user edanuff likes, use the following URL::


  http://api.usergrid.com/myapp/users/edanuff/likes


To limit returned likes to entities of type "restaurant", specify the following URL::


  http://api.usergrid.com/myapp/users/edanuff/likes/restaurant


-------------------------
Constructing API Requests
-------------------------


All Usergrid API requests are made to the sites http://api.usergrid.com or https://api.usergrid.com. 


Usergrid interprets the URL resource path as a list of names, UUIDs, or queries. The basic path format is::


  http://<hostname>/<app-uuid|app-name>/<collection-name>[/<entity-id|entity-name>]


This section gives many examples of how to construct Usergrid API requests. To focus on what's important, the examples use an abbreviated path that starts after the application UUID or application name. For example, instead of giving a fully qualified path name as in::


  http://api.usergrid.com/myapp/users


the example lists just this::


  /users


Remember, however, that HTTP requests must include the fully qualified URL, as well as an access token for authentication (in almost all cases).




Accessing Collections
---------------------


To access all entities in a collection, specify the path as follows::


  /users


Such a request retrieves the first 10 entities in the collection /users sorted by
their entity UUID. 






.. _queries:


Queries and Parameters
-----------------------


Often you'll want to issue a query that retrieves items from a collection. The format for queries is generally::


  /users?filter=querystring


For example, this request retrieves users whose first name is john::


  /users?filter=facebook.first_name%3D'john'


The querystring value is simply the URL-encoded version of the following::


  facebook.first_name='john'


You can construct multiple filters in which the results are ANDed together::


  /users?filter=facebook.first_name%3D'john'&filter=facebook.last_name%3D'doe'




Alternatives to URL Encoding
----------------------------


There are two alternatives to the standard format of URL encoding: an SQL-like query format and an optional type of JSON-encoded URL query. 


This conventional URL-encoded query retrieves a user entity named 'john.doe' if one exists::


  /users?filter=facebook.first_name%3D'john'&filter=facebook.last_name%3D'doe'


Alternatively, you can use an SQL-like query format::


  select * where facebook.first_name='john' and facebook.last_name='doe'


The URL-encoded version of this SQL-like query is::


  /users?ql=select * where facebook.first_name%3D'john' and facebook.last_name%3D'doe'




Another alternative to standard URL encoding is a JSON-encoded query, as in::


  /users/{"filter" : ["facebook.first_name = 'john'", "facebook.last_name = 'doe'"]}


This format is an optional, non-standard formatting convention. It is provided primarily to
simplify the process of constructing test queries. It is also used in the documentation to clarify some examples since URL-encoded query values are sometimes hard to follow. Although the Usergrid API can understand queries in this JSON-encoded format, it is recommended that you use conventionally formatted URL-encoded queries in client libraries. 


The Usergrid Developer Portal allows you to specify queries in all three formats, but transforms them into standard querystring URL-encoded parameters before issuing HTTP requests. 




Accessing Entities in a Collection
----------------------------------


To access a specific entity in a collection, simply specify its UUID or name. For example, to look up the user with a username of 'john.doe', you can issue either of these queries::
 
  /users/john.doe
  /users/1c18ca40-90b2-11e0-b91b-12313f0204bb


(This assumes, of course, that john.doe's UUID is '1c18ca40-90b2-11e0-b91b-12313f0204bb'.)


To retrieve the list of achievements for user "ed", specify::


  /users/ed/achievements


To retrieve the list of friends for the user "ed", enter::


  /users/ed/friends/


To obtain the achievements of friends of the user "ed", issue::


  /users/ed/friends/achievements


Query Parameters
----------------


The following query parameters can be passed in the querystring (or in a
JSON-encoded query):


==============  =======  ==========================================================
Parameter       Type     Description
==============  =======  ==========================================================
ql              string   a query in the query language
type            string   the entity type to return
reversed        string   return results in reverse order
connection      string   the connection type (e.g., “likes”)
start           string   the first entity’s UUID to return
cursor          string   an encoded representation of the query position for paging
limit           integer  the number of results to return
permission      string   a permission type
filter          string   a condition on which to filter 
==============  =======  ==========================================================


The logical place to put a query is in the querystring, but what happens when you want to
filter or query a collection somewhere other than at the end of the path? The
URI specification addresses this by allowing a form of embedded querystrings
inside the paths called matrix parameters.


In Usergrid, this URL path::


  /users/ed/friends;filter="location eq new york"/achievements?filter="level eq mayor"


is interpreted as this::


  /users/ed/friends/location="new york"/achievements/level="mayor"


Again, using JSON-encoded queries is optional and the URL path is human-readable::


  /users/ed/friends/{"filter" : "location='new york'"}/achievements/{"filter" : "level='mayor'"}


--------------------------
Format of Request Data
--------------------------


When creating or modifying entities, you supply information in the form
of a JSON object in the body of the HTTP request. You create an entity by POSTing to a collection and modify an entity by PUTting to an item in the collection.


When posting an entity, you provide the initial data you want for the entity::


  {
    "username" : "edanuff",
    "email" : "ed@anuff.com"
  }


You can create multiple entities in one request by supplying an array::


  [
    {
      "username" : "edanuff",
      "email" : "ed@anuff.com"
    },
    {
      "username" : "johndoe",
      "email" : "john.doe@gmail.com"
    }
  ]


--------------------------
Format of Response Data
--------------------------


All API methods return a response object that typically contains an array of
entities::


  {
    "entities" : [
      ...
    ]
  }


Not everything can be included inside the entity, and some of the data that
gets associated with specific entities isn't part of their persistent
representation. This is metadata, and it can be part of the response as well as
associated with a specific entity. Metadata is just an arbitrary key/value
JSON structure.


For example::


  {
    "entities" : {
      {
        "name" : "ed",
        "metadata" : {
          "collections" : ["activities", "groups", "followers"]
        }
      }
    },
    "metadata" : {
      "foo" : ["bar", "baz"]
    }
  }


Here's a full example of the response object with one entity in the response
(note that the Facebook property, which contains the entire Facebook profile
of the user, is not displayed in the example due to its size)::


  {
    "action" : "get",
    "application" : "ddde7630-90b1-11e0-b91b-12313f0204bb",
    "params" : { },
    "path" : "/users",
    "uri" : "http://api.usergrid.com/ddde7630-90b1-11e0-b91b-12313f0204bb/users",
    "entities" : [
      {
        "created" : 1307415547108000,
        "facebook" : { ... },
        "uuid" : "1c18ca40-90b2-11e0-b91b-12313f0204bb",
        "metadata" : {
          "path" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb",
          "collections" : {
            "activities" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/activities",
            "feed" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/feed",
            "groups" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/groups",
            "messages" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/messages",
            "queue" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/queue",
            "roles" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/roles",
            "following" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/following",
            "followers" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/followers"
          },
          "sets" : {
            "rolenames" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/rolenames",
            "permissions" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/permissions"
          }
        },
        "modified" : 1307415547108000,
        "name" : "John Doe",
        "picture" : "http://profile.ak.fbcdn.net/hprofile-ak-snc4/41501_217925_2656_q.jpg",
        "type" : "user",
        "username" : "john.doe"
      }
    ],
    "timestamp" : 1309218486419,
    "duration" : 40
  }