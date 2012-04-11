===============================
Group 
===============================


This section describes Usergrid endpoints relative to manipulating groups:


* :ref:`create-group`
* :ref:`add-user-group`
* :ref:`get-group`
* :ref:`update-group`
* :ref:`delete-user-group`
* :ref:`delete-group`


The Usergrid APIs create, retrieve, update, delete and query group entities. You can create 
application-specific group properties in addition to the system-defined group properties
that are described :ref:`here <group-properties>`.


.. _create-group:


-------------------
Create a New Group
-------------------


Create a new group. Groups use paths to indicate their unique names.  This allows you to
create group hierarchies by using slashes.  For this reason, a new group requires a “path”
property to be specified.


**Request URI**


  POST /{app_id}/groups


**Parameters**


 :arg uuid|string app_id: application UUID or application name


      
**Example - Request**


   .. code-block:: js


    POST /my-app/groups {"path":"mynewgroup"} 


**Example - Response**


   .. code-block:: js


    {
    "action": "post",
    "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
    "params": {},
    "path": "/groups",
    "uri": "https://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22/groups",
    "entities": [
        {
          "uuid": "a668717b-67cb-11e1-8223-12313d14bde7",
          "type": "group",
          "created": 1331066016571,
          "modified": 1331066016571,
          "metadata": {
            "path": "/groups/a668717b-67cb-11e1-8223-12313d14bde7",
            "sets": {
              "rolenames": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/rolenames"
            },
            "collections": {
              "activities": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/activities",
              "feed": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/feed",
              "roles": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/roles",
              "users": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users"
            }
          },
          "path": "mynewgroup"
        }
     ],
    "timestamp": 1331066016563,
    "duration": 35
    }






.. _add-user-group:


--------------------------
Add a User into a Group 
--------------------------


Adds a user to a group.


If the named group does not yet exist, an error message will be returned.


**Request URI**


  POST /{app_id}/groups/{uuid|groupname}/users/{uuid|username}


**Parameters**


 :arg uuid|string app_id: application UUID or application name
 :arg uuid|string groupname: UUID or name of the group
 :arg string "users": the collection "users"
 :arg uuid|string user: UUID or username of user


**Example - Request**


 .. code-block:: js


  POST /my-app/groups/mynewgroup/users/edanuff


**Example - Response**


 .. code-block:: js


  {
  "action": "post",
  "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
  "params": {},
  "path": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users",
  "uri": "https://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22/groups/a668717b-67cb-11e1-8223-12313d14bde7/users",
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
              "owners":   "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/connecting/owners"
               },
            "path": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22",
            "sets": {
              "rolenames": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/rolenames",
              "permissions": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/permissions"
               },
            "collections": {
              "activities": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/activities",
              "devices": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/devices",
              "feed": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/feed",
              "groups": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/groups",
              "roles": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/roles",
              "following": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/following",
              "followers": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/followers"
               }
          },
          "name": "Ed Anuff",
          "picture": "http://www.gravatar.com/avatar/90f823ba15655b8cc8e3b4d63377576f",
          "username": "edanuff"
        }
     ],
     "timestamp": 1331066031380,
     "duration": 64
  }


.. _get-group:


-------------------
Get a Group
-------------------


Get a group. 


**Request URI**


  GET /{app_id}/groups/{uuid|groupname}


**Parameters**


 :arg uuid|string app_id: application UUID or application name
 :arg uuid|string groupname: UUID or name of the group


      
**Example - Request**


   .. code-block:: js


    GET /my-app/groups/mynewgroup 


**Example - Response**


   .. code-block:: js


    {
    "action": "get",
    "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
    "params": {
          "_": [
            "1331066049869"
          ]
    },
    "path": "/groups",
    "uri": "https://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22/groups",
    "entities": [
        {
          "uuid": "a668717b-67cb-11e1-8223-12313d14bde7",
          "type": "group",
          "created": 1331066016571,
          "modified": 1331066016571,
          "metadata": {
            "path": "/groups/a668717b-67cb-11e1-8223-12313d14bde7",
            "sets": {
              "rolenames": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/rolenames"
            },
            "collections": {
              "activities": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/activities",
              "feed": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/feed",
              "roles": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/roles",
              "users": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users"
            }
          },
          "path": "mynewgroup"
        }
    ],
    "timestamp": 1331066050106,
    "duration": 18
    }




.. _update-group:

-------------------
Update a Group
-------------------


Update a group.  




**Request URI**


  PUT /{app_id}/groups/{uuid|groupname} {request body}


**Parameters**


 :arg uuid|string app_id: application UUID or application name
 :arg uuid|string groupname: UUID or name of the group
 :request body: a set of entity properties:
      
**Example - Request**


   .. code-block:: js


    PUT /my-app/groups/mynewgroup ("foo":"bar"}


**Example - Response**


   .. code-block:: js


    {
    "action": "put",
    "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
    "params": {},
    "path": "/groups",
    "uri": "https://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22/groups",
    "entities": [
        {
          "uuid": "a668717b-67cb-11e1-8223-12313d14bde7",
          "type": "group",
          "created": 1331066016571,
          "modified": 1331066092191,
          "foo": "bar",
          "metadata": {
            "path": "/groups/a668717b-67cb-11e1-8223-12313d14bde7",
            "sets": {
              "rolenames": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/rolenames"
            },
            "collections": {
              "activities": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/activities",
              "feed": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/feed",
              "roles": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/roles",
              "users": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users"
            }
          },
          "path": "mynewgroup"
        }
    ],
    "timestamp": 1331066092178,
    "duration": 31
    }




.. _delete-user-group:


--------------------------
Delete a User from a Group 
--------------------------


Delete a user from the specified group.




**Request URI**


  DELETE /{app_id}/groups/{uuid|groupname}/users/{uuid|username}


**Parameters**


 :arg uuid|string app_id: application UUID or application name
 :arg uuid|string groupname: UUID or name of the group
 :arg uuid|string user: UUID or username of user to be deleted


**Example - Request**


 .. code-block:: js


  DELETE /my-app/groups/mynewgroup/users/edanuff


**Example - Response**


 .. code-block:: js


  {
  "action": "delete",
  "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
  "params": {
        "_": [
          "1331066118009"
        ]
  },
  "path": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users",
  "uri": "https://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22/groups/a668717b-67cb-11e1-8223-12313d14bde7/users",
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
              "owners": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/connecting/owners"
            },
            "path": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22",
            "sets": {
              "rolenames": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/rolenames",
              "permissions": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/permissions"
            },
            "collections": {
              "activities": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/activities",
              "devices": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/devices",
              "feed": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/feed",
              "groups": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/groups",
              "roles": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/roles",
              "following": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/following",
              "followers": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users/6fbc8157-4786-11e1-b2bd-22000a1c4e22/followers"
            }
          },
          "name": "Ed Anuff",
          "picture": "http://www.gravatar.com/avatar/90f823ba15655b8cc8e3b4d63377576f",
          "username": "edanuff"
        }
  ],
  "timestamp": 1331066118193,
  "duration": 236
  }




.. _delete-group:


-------------------
Delete a Group
-------------------


Delete a group. 


**Request URI**


  DELETE /{app_id}/groups/{uuid|groupname}


**Parameters**


 :arg uuid|string app_id: application UUID or application name
 :arg uuid|string groupname: UUID or name of the group


      
**Example - Request**


   .. code-block:: js


    DELETE /my-app/groups/mynewgroup 


**Example - Response**


   .. code-block:: js


    {
    "action": "delete",
    "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
    "params": {
        "_": [
          "1331066144280"
        ]
    },
    "path": "/groups",
    "uri": "https://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22/groups",
    "entities": [
        {
          "uuid": "a668717b-67cb-11e1-8223-12313d14bde7",
          "type": "group",
          "created": 1331066016571,
          "modified": 1331066092191,
          "foo": "bar",
          "metadata": {
            "path": "/groups/a668717b-67cb-11e1-8223-12313d14bde7",
            "sets": {
              "rolenames": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/rolenames"
            },
            "collections": {
              "activities": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/activities",
              "feed": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/feed",
              "roles": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/roles",
              "users": "/groups/a668717b-67cb-11e1-8223-12313d14bde7/users"
            }
          },
          "path": "mynewgroup"
        }
    ],
    "timestamp": 1331066144462,
    "duration": 302
    }








.. _group-properties:


------------------------
Group Entity Properties
------------------------


Groups have the following properties:


============  =========  =========================================================
Property      Type       Description
============  =========  =========================================================
uuid          UUID       the group's unique entity id
type          string     "user"
created       long       UNIX timestamp of entity creation
modified      long       UNIX timestamp of entity modification
path          string     a valid slash-delimited group path (mandatory)
title         string     a display name
============  =========  =========================================================


Look-up properties for the entities of type “group” are UUID and path.


Groups have the following set properties:


============  =========  =========================================================
Set           Type       Description
============  =========  =========================================================
connections   string     set of connection types (e.g., "likes")
rolenames     string     set of roles assigned to a group
credentials   string     set of group credentials
============  =========  =========================================================


Groups have the following collections:


============  =========  =========================================================
Collection    Type       Description
============  =========  =========================================================
users         user       collection of users in the group
activities    activity   collection of activities a user has performed
feed          activity   inbox of activity notifications a group has received
roles         role       set of roles to which a group belongs
============  =========  =========================================================