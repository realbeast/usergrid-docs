==============
User 
==============


This section describes Usergrid endpoints relative to manipulating users:


* :ref:`create-user`
* :ref:`set-password`
* :ref:`get-user`
* :ref:`update-user`
* :ref:`delete-user`
* :ref:`query-users`
* :ref:`create-user-collections`
* :ref:`del-user-collections`
* :ref:`query-user-collections`


The Usergrid APIs create, retrieve, update, delete and query user entities. You can create 
application-specific user properties in addition to the system-defined user properties
that are described :ref:`here <user-properties>`.




.. _create-user:


-------------------
Create a New User
-------------------


Create a new user in the "users" collection. 


**Request URI**


  POST /{app_id}/users {request body}


**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :request body: one or more sets of user properties, of which "username" is mandatory and must be unique:


   .. code-block:: js


     {
       "username" : "john.doe",
       "email" : "john.doe@gmail.com",
       "name" : "John Doe",
       "password" : "test1234"
     }




**Example - Request**


   .. code-block:: js


    POST http://api.usergrid.com/my-app/users {"username":"john.doe","email":"john.doe@gmail.com","name":"John Doe"}
   
**Example - Response**


.. code-block:: js


   {
     "action" : "post",
     "application" : "4353136f-e978-11e0-8264-005056c00008",
     "params" : {
     },
     "path" : "/users",
     "uri" : "http://api.usergrid.com/4353136f-e978-11e0-8264-005056c00008/users",
     "entities" : [ {
       "uuid" : "7d1aa429-e978-11e0-8264-005056c00008",
       "type" : "user",
       "created" : 1317176452536016,
       "modified" : 1317176452536016,
       "activated" : true,
       "email" : "john.doe@gmail.com",
       "metadata" : {
         "path" : "/users/7d1aa429-e978-11e0-8264-005056c00008",
         "sets" : {
           "rolenames" : "/users/7d1aa429-e978-11e0-8264-005056c00008/rolenames",
           "permissions" : "/users/7d1aa429-e978-11e0-8264-005056c00008/permissions"
         },
         "collections" : {
           "activities" : "/users/7d1aa429-e978-11e0-8264-005056c00008/activities",
           "devices" : "/users/7d1aa429-e978-11e0-8264-005056c00008/devices",
           "feed" : "/users/7d1aa429-e978-11e0-8264-005056c00008/feed",
           "groups" : "/users/7d1aa429-e978-11e0-8264-005056c00008/groups",
           "roles" : "/users/7d1aa429-e978-11e0-8264-005056c00008/roles",
           "following" : "/users/7d1aa429-e978-11e0-8264-005056c00008/following",
           "followers" : "/users/7d1aa429-e978-11e0-8264-005056c00008/followers"
         }
       },
       "name" : "John Doe",
       "username" : "john.doe"
     } ],
     "timestamp" : 1317176452528,
     "duration" : 52
   }












.. _set-password:


-------------------
Set a User Password
-------------------


Creates a password for a user.


A password should be set when a user is first created. To protect password visibility, passwords are stored in a Usergrid credentials vault. 
Passwords must be at least 5 characters long and can include these characters: a-z, 0-9, @, #, $, %, ^,  and &.


If you are accessing an endpoint with Application-level access (rather than Application User-level access or anonymous access), 
then it is not necessary to provide an old password.


**Request URI**


  POST /{app_id}/users/{user}/password {request body}


**Parameters**


 :arg uuid|string app_id: application UUID or application name
 :arg string user: a user email or username
 :request body: the new and old passwords


 .. code-block:: js


  {"newpassword":"foo9876a","oldpassword":"bar1234b"}


**Example - Request**


 .. code-block:: js


  POST http://api.usergrid.com/my-app/users/john.doe/password {"newpassword":"foo9876a","oldpassword":"bar1234b"}


**Example - Response**


 .. code-block:: js


   {
     "action" : "post",
     "application" : "4353136f-e978-11e0-8264-005056c00008",
     "params" : {
     },
     "path" : "/users",
     "uri" : "http://api.usergrid.com/4353136f-e978-11e0-8264-005056c00008/users",
     "entities" : [ {
       "uuid" : "7d1aa429-e978-11e0-8264-005056c00008",
       "type" : "user",
       "created" : 1317176452536016,
       "modified" : 1317176452536016,
       "activated" : true,
       "email" : "john.doe@gmail.com",
       "metadata" : {
         "path" : "/users/7d1aa429-e978-11e0-8264-005056c00008",
         "sets" : {
           "rolenames" : "/users/7d1aa429-e978-11e0-8264-005056c00008/rolenames",
           "permissions" : "/users/7d1aa429-e978-11e0-8264-005056c00008/permissions"
         },
         "collections" : {
           "activities" : "/users/7d1aa429-e978-11e0-8264-005056c00008/activities",
           "devices" : "/users/7d1aa429-e978-11e0-8264-005056c00008/devices",
           "feed" : "/users/7d1aa429-e978-11e0-8264-005056c00008/feed",
           "groups" : "/users/7d1aa429-e978-11e0-8264-005056c00008/groups",
           "roles" : "/users/7d1aa429-e978-11e0-8264-005056c00008/roles",
           "following" : "/users/7d1aa429-e978-11e0-8264-005056c00008/following",
           "followers" : "/users/7d1aa429-e978-11e0-8264-005056c00008/followers"
         }
       },
       "name" : "John Doe",
       "username" : "john.doe"
     } ],
     "timestamp" : 1329429630637,
     "duration" : 100
   }




.. _get-user:


------------------------------
Get a User by UUID or Username
------------------------------


Retrieves a user given a specified UUID or username.


Rather than specifying a UUID, you can provide a unique "username" to retrieve a user object. 


**Request URI**


   GET /{app_id}/users/{uuid|username} 


**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :arg uuid|string uuid|name: a user UUID or username


**Example - Request**


   .. code-block:: js


    GET http://api.usergrid.com/my-app/users/jane.doe


or


 .. code-block:: js


    GET http://api.usergrid.com/my-app/users/a407b1e7-58e8-11e1-ac46-22000a1c5a67






**Example - Response**


.. code-block:: js
    
  {
 "action": "get",
 "application": "1c8f60e4-da67-11e0-b93d-12313f0204bb",
 "params": {
   "_": [
     "1315524419746"
   ]
 },
 "path": "/users",
 "uri": "http://api.usergrid.com/1c8f60e4-da67-11e0-b93d-12313f0204bb/users",
 "entities": [
   {
     "uuid": "78c54a82-da71-11e0-b93d-12313f0204bb",
     "type": "user",
     "created": 1315524171347008,
     "modified": 1315524171347008,
     "email": "jane.doe@gmail.com",
     "metadata": {
       "path": "/users/78c54a82-da71-11e0-b93d-12313f0204bb",
       "collections": {
         "activities": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/activities",
          "devices": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/devices",
         "feed": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/feed",
         "groups": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/groups",
         "messages": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/messages",
         "queue": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/queue",
         "roles": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/roles",
         "following": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/following",
         "followers": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/followers"
       },
       "sets": {
         "rolenames": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/rolenames",
         "permissions": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/permissions"
       }
     },
     "username": "jane.doe"
   }
 ],
 "timestamp": 1315524421071,
 "duration": 107
 }
 




.. _update-user:


----------------
Update a User
----------------


Update a user with new or changed property values.


**Request URI**


  PUT /{app_id}/users/{uuid|username} {request body} 


**Parameters**


 :arg uuid|string app_id: application UUID or application name
 :arg uuid|string uuid|username: a user UUID or username
 :request body: one or more sets of user properties:


 .. code-block:: js


     {
       "email" : "jdoe57@gmail.com",
       "city" : "san francisco"
     }


**Example - Request**


 .. code-block:: js


  PUT http://api.usergrid.com/my-app/users/jane.doe {"city":"san francisco"}
     






**Example - Response**


.. code-block:: js


  {
 "action": "put",
 "application": "1c8f60e4-da67-11e0-b93d-12313f0204bb",
 "params": {},
 "path": "/users",
 "uri": "http://api.usergrid.com/1c8f60e4-da67-11e0-b93d-12313f0204bb/users",
 "entities": [
   {
     "uuid": "78c54a82-da71-11e0-b93d-12313f0204bb",
     "type": "user",
     "created": 1315524171347008,
     "modified": 1315524526405008,
     "city": "san francisco",
     "email": "jdoe57@gmail.com",
     "metadata": {
       "path": "/users/78c54a82-da71-11e0-b93d-12313f0204bb",
       "collections": {
         "activities": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/activities",
          "devices": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/devices",
         "feed": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/feed",
         "groups": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/groups",
         "messages": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/messages",
         "queue": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/queue",
         "roles": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/roles",
         "following": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/following",
         "followers": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/followers"
       },
       "sets": {
         "rolenames": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/rolenames",
         "permissions": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/permissions"
       }
     },
     "username": "jane.doe"
   }
 ],
 "timestamp": 1315524526343,
 "duration": 84
 }












.. _delete-user:


---------------------------------------
Delete a User
---------------------------------------


Deletes the specified user given the UUID or username.


Returns the contents of the deleted user.


**Request URI**


  DELETE /{app_id}/users/{uuid|username} 


**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :arg uuid|string uuid|username: a user UUID or username


**Example - Request**


   .. code-block:: js


    DELETE http://api.usergrid.com/my-app/users/ {"username":"john.doe"}


**Example - Response**
     
.. code-block:: js
    
 {
   "action": "delete",
   "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
   "params": {
     "_": [ 
     "1329432186755" ]
      },
   "path": "/users",
   "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/users",
   "entities": [
      {
     "uuid": "78c54a82-da71-11e0-b93d-12313f0204bb",
     "type": "user",
     "created": 1315524171347008,
     "modified": 1315524526405008,
     "city": "san francisco",
     "email": "jdoe57@gmail.com",
     "metadata": {
       "path": "/users/78c54a82-da71-11e0-b93d-12313f0204bb",
       "collections": {
         "activities": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/activities",
          "devices": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/devices",
         "feed": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/feed",
         "groups": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/groups",
         "messages": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/messages",
         "queue": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/queue",
         "roles": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/roles",
         "following": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/following",
         "followers": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/followers"
     },
     "sets": {
         "rolenames": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/rolenames",
         "permissions": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/permissions"
       }
     },
     "username": "jane.doe"
   }
 ],
 "timestamp": 1315524526343,
 "duration": 84
 }




.. _query-users:


----------------------------
Query to Get Users
----------------------------


Retrieves the set of users that meet the query criteria or a maximum of 10 users if no query filters are provided. See :ref:`queries` for details on options for querying or filtering.


**Request URI**


    GET /{app_id}/users?ql=&reversed=&start=&cursor=&limit=&permission=&filter=


**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :optparam string ql: a query in the query language
   :optparam string reversed: return results in reverse order
   :optparam uuid start: the first user UUID to return
   :optparam string cursor: an encoded representation of the query position for paging
   :optparam integer limit: the number of results to return (default=10)
   :optparam string permission: a permission type
   :optparam string filter: a condition on which to filter (multiple conditions are allowed)


**Example - Request**


   .. code-block:: js


    GET http://api.usergrid.com/my-app/users?filter=city%3D'chicago'


**Example - Response**


.. code-block:: js


  {
 "action": "get",
 "application": "1c8f60e4-da67-11e0-b93d-12313f0204bb",
 "params": {
    "filter": [ 
      "city='chicago'?_=1329433677419" 
    ]
    },
 "path": "/users",
 "uri": "http://api.usergrid.com/1c8f60e4-da67-11e0-b93d-12313f0204bb/users",
 "entities": [
   {
     "uuid": "78c54a82-da71-11e0-b93d-12313f0204bb",
     "type": "user",
     "created": 1315524171347008,
     "modified": 1315524526405008,
     "city": "chicago",
     "email": "jdoe57@gmail.com",
     "metadata": {
       "path": "/users/78c54a82-da71-11e0-b93d-12313f0204bb",
       "collections": {
         "activities": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/activities",
          "devices": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/devices",
         "feed": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/feed",
         "groups": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/groups",
         "messages": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/messages",
         "queue": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/queue",
         "roles": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/roles",
         "following": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/following",
         "followers": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/followers"
         },
       "sets": {
         "rolenames": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/rolenames",
         "permissions": "/users/78c54a82-da71-11e0-b93d-12313f0204bb/permissions"
       }
     },
     "username": "jane.doe"
   }
   ],
   "timestamp": 1329433677599,
   "duration": 41
 }






.. _create-user-collections: 


------------------------------------------------------
Add a User into a Collection or Create a Connection
------------------------------------------------------
Posts a user to a collection or creates a relationship between a user and another entity.


There are two common use cases, each described separately.


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


If the relationship is a collection (such as a "users" collection for the group "employees"), this 
request adds the second entity to the first entity's collection of the specified name. 
In the case of a group, this is how you add users as group members.


If the relationship is not defined for the entity, a connection is created. For example, 
if the relationship is defined as "likes", the second entity is connected to the first with a "likes" relationship:


  .. code-block:: js


   POST http://api.usergrid.com/my-app/users/jane.doe/likes/4c469e8a-d8ed-11e0-bcc1-12313f0204bb




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


   POST http://api.usergrid.com/my-app/users/john.doe/likes/food/pizza


When creating a connection, if you specify the entity type for the second
entity, then you can create the connection using the entity's name rather than its UUID. 


**Example - Response**


 .. code-block:: js


   { 
   "action": "post", 
   "application": "3e163873-6725-11e1-8223-12313d14bde7", 
   "params": {
      }, 
   "path": "/users/e1ada559-6725-11e1-8223-12313d14bde7/likes", 
   "uri": "http://api.usergrid.com/3e163873-6725-11e1-8223-12313d14bde7/users/e1ada559-6725-11e1-8223-12313d14bde7/likes", 
   "entities": [], 
   "timestamp": 1331075231504, 
   "duration": 42 
   }








.. _del-user-collections: 


------------------------------------------------------
Delete a User from a Collection or Delete a Connection
------------------------------------------------------
Deletes a user from a collection or deletes a relationship between a user and another entity.


There are two common use cases, each described separately.


**Request URI**


    DELETE /{app_id}/{collection}/{first_entity_id}/{relationship}/{second_entity_id}


**Parameters - Use Case 1**


  :arg uuid|string app_id: application UUID or application name
  :arg string collection: a collection name
  :arg uuid|string first_entity_id: entity UUID or entity name
  :arg string relationship: a collection name or connection type (e.g., "likes")
  :arg uuid second_entity_id: entity UUID


**Example - Request - Use Case 1**


  .. code-block:: js


   DELETE http://api.usergrid.com/my-app/groups/employees/users/jane.doe


If the relationship is a collection (such as a "users" collection for the group "employees"), this 
request removes the second entity from the first entity's collection of the specified name. 
In the case of a group, this is how you delete users as group members.


For connections, you provide the connection type. For example, if the second entity is connected to 
the first with a "likes" relationship, the delete request removes the connection:


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


   DELETE /users/test-user-1/follows/test-user-2


When removing a connection, if you specify the entity type for the second
entity, then you can delete the connection using the entity's name rather than its UUID. 


**Example - Response**


 .. code-block:: js


  {
   "action": 
   "delete", 
   "application": "3e163873-6725-11e1-8223-12313d14bde7", 
   "params": { 
      "_": [
        "1331075473054" 
     ] 
   }, 
   "path": "/users/e1ada559-6725-11e1-8223-12313d14bde7/follows", 
   "uri": "http://api.usergrid.com/3e163873-6725-11e1-8223-12313d14bde7/users/e1ada559-6725-11e1-8223-12313d14bde7/follows", 
   "entities": [], 
   "timestamp": 1331075473194, 
   "duration": 22 
  }






.. _query-user-collections: 


--------------------------------------------
Query a User's Collections or Connections
--------------------------------------------
Retrieves the set of users that meet the query criteria or a maximum of up to 10 entities if no query filters are provided. 


See :ref:`queries` for a discussion of options for querying and filtering.


**Request URI**


    GET /{app_id}/users/{uuid|username}/{relationship}?ql=&type=&reversed=&start=&cursor=&limit=&permission=&filter=


**Parameters**


   :arg uuid|string app_id: application UUID or application name
   :arg string "users": a collection name, in this case "users"
   :arg uuid|string username: entity UUID or username 
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


    GET http://api.usergrid.com/myapp/users/jane.doe/likes?filter=city%3D'milwaukee'


**Example - Response**


   .. code-block:: js


      {
        "action": "get",
        "application": "5ea08de5-4d23-11e1-b41d-22000a1c4e22",
        "params": { 
          "filter": [ 
            "city='milwaukee'?_=1329494905832" 
            ]
          },
        "path": "/users/b7874124-58f2-11e1-ac46-22000a1c5a67/likes",
        "uri": "http://api.usergrid.com/5ea08de5-4d23-11e1-b41d-22000a1c4e22/users/b7874124-58f2-11e1-ac46-22000a1c5a67/likes",
        "entities": [
          {
            "uuid": "895d7c11-5901-11e1-ac46-22000a1c5a67",
            "type": "restaurant",
            "created": 1329439893227,
            "modified": 1329439893227,
            "city": "milwaukee",
            "metadata": {
              "connecting": { 
                "likes": "/users/b7874124-58f2-11e1-ac46-22000a1c5a67/likes/895d7c11-5901-11e1-ac46-22000a1c5a67/connecting/likes" 
                }, 
                "connection": "likes", 
                "cursor": "gGkAAQEAgHMACW1pbHdhdWtlZQCAdQAQiV18EVkBEeGsRiIAChxaZwCAcwAKcmVzdGF1cmFudACAdQAQUrM7mVkCEeGsRiIAChxaZwA", 
                "associated": "42039286-c984-3b46-bbfb-28c4abb9b27c", 
                "path": "/users/b7874124-58f2-11e1-ac46-22000a1c5a67/likes/895d7c11-5901-11e1-ac46-22000a1c5a67" 
            }, 
            "name": "Tulep" 
          }
        ]        
        "timestamp": 1315357451949,
        "duration": 52
      }






.. _user-properties:


----------------------
User Entity Properties
----------------------


You can create application-specific user properties in addition to these system-defined user properties:
        
============  =========  ===========================================================
Property      Type       Description
============  =========  ===========================================================
uuid          UUID       the user's unique entity id
type          string     the type of entity, in this case "user"
created       long       UNIX timestamp of entity creation
modified      long       UNIX timestamp of entity modification
username      string     a valid and unique string username (mandatory)
email         string     a valid and unique email address
name          string     user display name
activated     boolean    whether the user account has been activated
disabled      boolean    whether the user account has been administratively disabled
firstname     string     user first name
middlename    string     user middle name
lastname      string     user last name
picture       string     user picture
============  =========  ===========================================================




The following property sets are assigned to user entities:


============  =========  =========================================================
Set           Type       Description
============  =========  =========================================================
connections   string     set of connection types (e.g., "likes", etc.)
rolenames     string     set of roles assigned to a user
permissions   string     set of user permissions
credentials   string     set of user credentials
============  =========  =========================================================




Users have the following associated collections:


============  =========  =========================================================
Collection    Type       Description
============  =========  =========================================================
groups        group      collection of groups to which a user belongs 
devices       device     collection of devices in the service  
activities    activity   collection of activities a user has performed
feed          activity   inbox of activity notifications a user has received
roles         role       set of roles assigned to a user
============  =========  =========================================================