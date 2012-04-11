.. _gen-purpose-endpoints:


===============================
General Purpose Endpoints
===============================


This section describes Usergrid endpoints for managing entities other than users, groups, and roles:


* :ref:`create-entity`
* :ref:`get-entity`
* :ref:`update-entity`
* :ref:`delete-entity`
* :ref:`query-collection`
* :ref:`update-collection-query`
* :ref:`query-entity-collection`
* :ref:`put-entity-collection`
* :ref:`del-entity-collection`




.. _create-entity:


--------------------------------------
Create a New Entity or Collection
--------------------------------------


Post an entity to the specified collection. 
   
Collections do not need to be defined before being used; simply posting an entity to a collection creates 
the collection if it doesn't already exist.


**Request URI**


    POST /{app_id}/{collection}{request body}




**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :request body: either a single property or set of entity properties:


   .. code-block:: js


     { "foo" : "bar", "a" : { "b" : 1, "c" : 2 } }


   or an array to create multiple entities:


   .. code-block:: js


     [ { "foo" : "bar" }, { "foo" : "baz"} ]


**Example - Request**


   .. code-block:: js


    POST http://api.usergrid.com/my-app/cats {"name":"nico"}


**Example - Response**


   .. code-block:: js


    {
    "action": "post",
    "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
    "params": {},
    "path": "/cats",
    "uri": "https://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22/cats",
    "entities": [
           {
            "uuid": "1a7c2177-67cb-11e1-8223-12313d14bde7",
            "type": "cat",
            "created": 1331065781819,
            "modified": 1331065781819,
            "metadata": {
              "path": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7"
            },
            "name": "nico"
          }
    ],
    "timestamp": 1331065781798,
    "duration": 74
    }


Entity contents are specified in JSON format. Any valid JSON object can be stored in an entity, regardless of the level of complexity, and can be more than just name/value pairs. Entity types are defined by the type of collection to which they are posted.  For example, posting to a collection named "cats" creates entities of type "cat".


*All user-defined properties are indexed.* Entities are fully indexed, and strings that contain multiple words are keyword-indexed as well.


For all application-specific entities, you can provide a property called "name" that you can use to retrieve the entity rather than its UUID. The value for the "name" property must be unique. Some predefined entities (like "user") specify a different look-up property that can be used for identifying an entity. For example, user entities specify the "username" property rather than "name". Refer to specific system-defined entity descriptions for details.












.. _get-entity:


------------------------------------
Get an Entity by Entity UUID or Name
------------------------------------


Gets an entity given the specified UUID or entity lookup property.


For any application-specific entity type, if there is a "name" property specified for the entity, you can specify "name" instead of the UUID to retrieve the entity. For system-provided entities, there may be a different property than "name" defined for doing look-ups. For example, the "user" entity defines "username" as its look-up property. Refer to the entity descriptions to see if there is an alternate look-up property defined for a system-provided entity type.


**Request URI**


    GET /{app_id}/{collection}/{uuid|name}


**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string uuid|name: an entity UUID or name


When the entity is a user, use “username” instead of “name” to retrieve the entity.


**Example - Request**


   .. code-block:: js


    GET http://api.usergrid.com/my-app/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb


**Example - Response**


   .. code-block:: js


    {
        "action": "get",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": { 
          "_": [
            "1329765805584" ] 
        }
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }






.. _update-entity:


----------------
Update an Entity
----------------


Update an entity in a collection.  


New property values are stored in the entity.


**Request URI**


  PUT /{app_id}/{collection}/{uuid|name}{request body}


**Parameters**


 :arg uuid|string app_id: application UUID or application name
 :arg string collection: a collection name
 :arg uuid|string uuid|name: an entity UUID or name
 :request body: a set of entity properties:


 .. code-block:: js


   {"alpha":"bravo"}


**Example - Request**


 .. code-block:: js


  PUT http://api.usergrid.com/my-app/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb/{"alpha":"bravo"}


**Example - Response**


  .. code-block:: js


    {
        "action": "put",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "alpha": "bravo",
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }




________________


.. _delete-entity:


---------------------------------------
Delete an Entity by Entity UUID or Name
---------------------------------------


Deletes the message with the specified UUID or entity look-up name.


Returns the contents of the deleted entity.


**Request URI**


    DELETE /{app_id}/{collection}/{uuid|name}


**Parameters**
   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string uuid|name: an entity UUID or name


**Example - Request**


   .. code-block:: js


    DELETE http://api.usergrid.com/my-app/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb


**Example - Response**
   
   .. code-block:: js


    {
        "action": "delete",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }




.. _query-collection:


------------------
Query a Collection
------------------


Retrieves the set of entities that meet the query criteria or a maximum of 10 entities if no query filters are provided. See :ref:`queries` for details on options for querying or filtering.


**Request URI**


    GET /{app_id}/{collection}?ql=&reversed=&start=&cursor=&limit=&permission=&filter=


**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :optparam string ql: a query in the query language
   :optparam string reversed: return results in reverse order
   :optparam uuid start: the first entity UUID to return
   :optparam string cursor: an encoded representation of the query position for paging
   :optparam integer limit: the number of results to return (default=10)
   :optparam string permission: a permission type
   :optparam string filter: a condition on which to filter (multiple conditions are allowed)


**Example - Request**


   .. code-block:: js


    GET http://api.usergrid.com/my-app/things?filter=foo%3D'bar'


**Example - Response**
 
   .. code-block:: js


    {
        "action": "get",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": { 
          "filter": [ 
            "foo='bar'?_=1329766451716" 
          ] 
        },
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "foo": "bar",
                "metadata": {
                    "cursor": "gGkAAQEAgHMAA2JhcgCAdQAQf6_KZ1vpEeGsRiIAChxaZwCAdQAQf6-jVlvpEeGsRiIAChxaZwA",
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }


 


.. _update-collection-query:


----------------------------
Update a Collection by Query
----------------------------


Updates all entities that meet the specified criteria.


**Request URI**
  
   PUT /{app_id}/{collection}?ql=&reversed=&permission=&filter={request body}


**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :optparam string ql: a query in the query language
   :optparam string permission: a permission type
   :optparam string filter: a condition on which to filter (multiple conditions are allowed)
   :request body: a set of entity properties:


   .. code-block:: js


     {"alpha":"bravo"}


**Example - Request**


   .. code-block:: js


    PUT http://api.usergrid.com/my-app/things&filter=foo%3D'bar'{"alpha":"bravo"}


**Example - Response**


   .. code-block:: js


    {
        "action": "put",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "alpha": "bravo",
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }






________________


.. _query-entity-collection:


--------------------------------------------
Query an Entity's Collections or Connections
--------------------------------------------
Retrieves the set of entities that meet the query criteria or a maximum of up to 10 entities if no query filters are provided. 


See :ref:`queries` for details on options for querying or filtering.


**Request URI**


    GET /{app_id}/{collection}/{entity_id}/{relationship}?ql=&type=&reversed=&start=&cursor=&limit=&permission=&filter=


**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (e.g., "likes")
   :optparam string ql: a query in the query language
   :optparam string type: the entity type to return
   :optparam string reversed: return results in reverse order
   :optparam string connection: the connection type (e.g., "likes")
   :optparam uuid start: the first entity UUID to return
   :optparam string cursor: an encoded representation of the query position for paging
   :optparam integer limit: the number of results to return (default=10)
   :optparam string permission: a permission type
   :optparam string filter: a condition on which to filter (multiple conditions are allowed)


**Example - Request**


   .. code-block:: js


    GET http://api.usergrid.com/my-app/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb/likes


**Example - Response**


   .. code-block:: js


    {
        "action": "get",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "alpha": "bravo",
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }






.. _put-entity-collection:


------------------------------------------------------
Put an Entity Into a Collection or Create a Connection
------------------------------------------------------
Posts an entity to a collection or creates a connection between entities.


There are two use cases, each described separately.


**Request URI**


    POST /{app_id}/{collection}/{first_entity_id}/{relationship}/{second_entity_id}


**Parameters - Use Case 1**


   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string first_entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (e.g., "likes")
   :arg uuid second_entity_id: entity UUID


**Example - Request - Use Case 1**


   .. code-block:: js


    POST http://api.usergrid.com/my-app/groups/employees/users/jane.doe


If relationship is a collection (such as a "users" collection for a group), this request adds the second entity to the first entity's collection of the specified name. In the case of a group, this is how you add users as group members.


If relationship is not defined for the entity, a connection is created. For example, if the relationship is defined as "likes", the second entity is connected to the first with a "likes" relationship:


   .. code-block:: js


    POST http://api.usergrid.com/my-app/users/ed@anuff.com/likes/4c469e8a-d8ed-11e0-bcc1-12313f0204bb




----------


**Request URI**


    POST /{app_id}/{collection}/{first_entity_id}/{relationship}/{second_entity_type}/{second_entity_id}


**Parameters - Use Case 2**


   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string first_entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (e.g., "likes")
   :arg string second_entity_type: the type of the second entity
   :arg uuid|string second_entity_id: entity UUID or entity name


**Example - Request - Use Case 2**


   .. code-block:: js


    POST http://api.usergrid.com/my-app/cats/nico/owners/user/edanuff


When creating a connection, if you specify the entity type for the second 
entity, then you can create the connection using the entity's name rather than its UUID.  


**Example - Response - Use Case 2**


   .. code-block:: js


    {
    "action": "post",
    "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
    "params": {},
    "path": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners",
    "uri": "https://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners",
    "entities": [
        {
          "uuid": "6fbc8157-4786-11e1-b2bd-22000a1c4e22",
          "type": "user",
          "created": 1327517852364015,
          "modified": 1327517852364015,
          "activated": true,
          "email": "ed@anuff.com",
          "metadata": {
            "path": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22",
            "sets": {
              "rolenames": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/rolenames",
              "permissions": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/permissions"
            },
            "collections": {
              "activities": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/activities",
              "devices": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/devices",
              "feed": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/feed",
              "groups": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/groups",
              "roles": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/roles",
              "following": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/following",
              "followers": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/followers"
            }
          },
          "name": "Ed Anuff",
          "picture": "http://www.gravatar.com/avatar/90f823ba15655b8cc8e3b4d63377576f",
          "username": "edanuff"
        }
     ],
    "timestamp": 1331065802496,
    "duration": 85
    }






.. _del-entity-collection:


---------------------------------------------------------
Remove an Entity from a Collection or Delete a Connection
---------------------------------------------------------
Removes an entity from a collection or deletes a connection between entities.


There are two use cases, each described separately.


**Request URI**


    DELETE /{app_id}/{collection}/{first_entity_id}/{relationship}/{second_entity_id}


**Parameters - Use Case 1**


   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string first_entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (e.g., "likes")
   :arg uuid second_entity_id: entity UUID or entity name


**Example - Request - Use Case 1**


   .. code-block:: js


    DELETE http://api.usergrid.com/my-app/groups/employees/users/jane.doe


If relationship is a collection (such as a "users" collection for the group "employees"), the request removes the second entity from the specified first entity’s collection. In the case of the group "employees", users are removed from the group.


For connections, you provide the connection type. For example, if the second entity is connected to the first with a "likes" relationship, the delete request removes the connection:


   .. code-block:: js


    DELETE http://api.usergrid.com/my-app/users/ed@anuff.com/likes/4c469e8a-d8ed-11e0-bcc1-12313f0204bb




----------


**Request URI**


    DELETE /{app_id}/{collection}/{first_entity_id}/{relationship}/{second_entity_type}/{second_entity_id}


**Parameters - Use Case 2**


   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string first_entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (e.g., "likes")
   :arg string second_entity_type: the type of the second entity
   :arg uuid|string second_entity_id: entity UUID or entity name


**Example - Request - Use Case 2**


   .. code-block:: js


    DELETE http://api.usergrid.com/my-app/cats/nico/owners/user/edanuff


When deleting a connection, if you specify the entity type for the second entity, then you can delete the connection using the entity's name rather than its UUID.  


**Example - Response - Use Case 2**


  .. code-block:: js


   {
   "action": "delete",
   "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
   "params": {
        "_": [
          "1331065920372"
        ]
   },
   "path": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners",
   "uri": "https://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners",
   "entities": [
        {
          "uuid": "6fbc8157-4786-11e1-b2bd-22000a1c4e22",
          "type": "user",
          "created": 1327517852364015,
          "modified": 1327517852364015,
          "activated": true,
          "email": "ed@anuff.com",
          "metadata": {
            "connecting": {
              "owners": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/connecting/owners"
            },
            "path": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22",
            "sets": {
              "rolenames": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/rolenames",
              "permissions": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/permissions"
            },
            "collections": {
              "activities": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/activities",
              "devices": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/devices",
              "feed": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/feed",
              "groups": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/groups",
              "roles": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/roles",
              "following": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/following",
              "followers": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7/owners/6fbc8157-4786-11e1-b2bd-22000a1c4e22/followers"
            }
          },
          "name": "Ed Anuff",
          "picture": "http://www.gravatar.com/avatar/90f823ba15655b8cc8e3b4d63377576f",
          "username": "edanuff"
        }
    ],
   "timestamp": 1331065921042,
   "duration": 45
   }