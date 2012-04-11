==================
Role 
==================


This section describes endpoints that manipulate roles:


* :ref:`create-role`
* :ref:`get-roles`
* :ref:`delete-role`
* :ref:`get-perm-app-roles`
* :ref:`add-perm-app-roles`
* :ref:`delete-perm-app-roles`


The Usergrid APIs create, retrieve, update, delete and query roles. You can create application-specific role properties in addition to the system-defined role properties
that are described :ref:`here <role-properties>`. 


For an introduction to roles and their use, see :ref:`permissions-section`.


.. _create-role:


-----------------
Create a New Role
-----------------


Creates a new application role.


**Request URI**


 POST /{app_id}/rolenames {request body}


**Parameters**


  :arg uuid|string app_id: application UUID or application name
  :request body: the role name and title:


  .. code-block:: js


    { "name" : "manager", "title" : "Manager" }


**Example - Request**


  .. code-block:: js


     POST http://api.usergrid.com/my-app/rolenames/ {"name":"manager","title":"Manager"} 


**Example - Response**


.. code-block:: js


   {
     "action" : "post",
     "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "params" : {
       "_" : [ "1328058070002" ]
     },
     "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "entities" : [ ],
     "data" : {
       "admin" : "Administrator",
       "default" : "Default",
       "manager" : "Manager",
       "guest" : "Guest"
     },
     "timestamp" : 1328058069799,
     "duration" : 46
   }




.. _get-roles:


-------------------------------
Get the Roles in an Application
-------------------------------


Gets the roles for a specific application.


**Request URI**


 GET /{app_id}/rolenames


**Parameters**


  :arg uuid|string app_id: application UUID or application name


**Example - Request**


.. code-block:: js


   GET http://api.usergrid.com/my-app/rolenames


**Example - Response**
  
.. code-block:: js


   {
     "action" : "get",
     "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "params" : {
       "_" : [ "1328058070002" ]
     },
     "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "entities" : [ ],
     "data" : {
       "admin" : "Administrator",
       "default" : "Default",
       "guest" : "Guest"
     },
     "timestamp" : 1328058069799,
     "duration" : 46
   }


.. _delete-role:


--------------
Delete a Role
--------------


Deletes the specified role and returns the revised set of application roles.


**Request URI**


  DELETE /{app_id}/rolenames/{rolename}


**Parameters**


  :arg uuid|string app_id: application UUID or application name
  :arg string rolename: a role name


**Example - Request**


 .. code-block:: js


  DELETE http://api.usergrid.com/my-app/rolenames/manager


**Example - Response**


.. code-block:: js


     {
     "action" : "delete",
     "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "params" : {
       "_" : [ "1328058070002" ]
     },
     "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "entities" : [ ],
     "data" : {
       "admin" : "Administrator",
       "default" : "Default",
       "guest" : "Guest"
     },
     "timestamp" : 1328058069799,
     "duration" : 46
   }




.. _get-perm-app-roles:


---------------------------------------
Get Permissions for an Application Role
---------------------------------------


Gets permissions for the specific app role.


**Request URI**


   GET /{app_id}/rolenames/{rolename}


**Parameters**


  :arg uuid|string app_id: application UUID or application name
  :arg string rolename: a role name


**Example - Request**


  .. code-block:: js


      GET http://api.usergrid.com/my-app/rolenames/manager


**Example - Response**


.. code-block:: js


   {
     "action" : "get",
     "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "params" : {
       "_" : [ "1328058543902" ]
     },
     "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "entities" : [ ],
     "data" : [
       "get,put,post,delete:/users/${user}",
       "get,put,post,delete:/users/${user}/activities",
       "get,put,post,delete:/users/${user}/feed",
       "get,put,post,delete:/users/${user}/following/*",
       "get,put,post,delete:/users/${user}/following/user/*",
       "get,put,post,delete:/users/${user}/groups"
     ],
     "timestamp" : 1328058543530,
     "duration" : 33
   }






.. _add-perm-app-roles:


--------------------------------------
Add Permissions to an Application Role
--------------------------------------


Adds permissions for the specified application role.


**Request URI**


   POST /{app_id}/rolenames/{rolename} {request body}


**Parameters**


  :arg uuid|string app_id: application UUID or application name
  :arg string rolename: a role name
  :request body: a JSON object with a role name and title:


  .. code-block:: js


    { "permission" : "get,put,post,delete:/users/${user}/groups" }


**Example - Request**


  .. code-block:: js


   POST http://api.usergrid.com/my-app/rolenames/manager {"permission":"get,put,post,delete:/users/${user}/groups" }


**Example - Response**


.. code-block:: js


   {
     "action" : "post",
     "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "params" : {
       "_" : [ "1328058543902" ]
     },
     "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "entities" : [ ],
     "data" : [
       "get,put,post,delete:/users/${user}",
       "get,put,post,delete:/users/${user}/activities",
       "get,put,post,delete:/users/${user}/feed",
       "get,put,post,delete:/users/${user}/following/*",
       "get,put,post,delete:/users/${user}/following/user/*",
       "get,put,post,delete:/users/${user}/groups"
     ],
     "timestamp" : 1328058543530,
     "duration" : 33
   }






________________


.. _delete-perm-app-roles:


---------------------------------------------
Delete Permissions from an Application Role
---------------------------------------------


Removes permissions from the specified application role.


**Request URI**


   DELETE /{app_id}/rolenames/{rolename}?permission={permission}


**Parameters**


  :arg uuid|string app_id: application UUID or application name
  :arg string rolename: a role name
  :arg string permission: a permission


**Example - Request**


  .. code-block:: js


   DELETE http://api.usergrid.com/my-app/rolenames/manager?permission=delete


**Example - Response**


.. code-block:: js


   {
     "action" : "delete",
     "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "params" : {
       "_" : [ "1328058543902" ]
     },
     "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
     "entities" : [ ],
     "data" : [
       "get,put,post,delete:/users/${user}",
       "get,put,post,delete:/users/${user}/activities",
       "get,put,post,delete:/users/${user}/feed",
       "get,put,post,delete:/users/${user}/following/*",
       "get,put,post,delete:/users/${user}/following/user/*",
     ],
     "timestamp" : 1328058543530,
     "duration" : 33
   }




.. _role-properties:


----------------
Role Properties
----------------


Roles have the following properties:
        
============  =========  =========================================================
Property      Type       Description
============  =========  =========================================================
uuid          UUID       the role's unique entity id
type          string     "role"
created       long       UNIX timestamp of entity creation
modified      long       UNIX timestamp of entity modification
name          string     a unique name identifying the role (mandatory)
roleName      string     an informal role name
title         string     a role title
============  =========  =========================================================


The look-up proprety for a role is “name”.
Roles have the following set properties:


============  =========  =========================================================
Set           Type       Description
============  =========  =========================================================
permissions   string     set of user permissions
============  =========  =========================================================




Roles have the following collections:


============  =========  =========================================================
Collection    Type       Description
============  =========  =========================================================
users         user       collection of users assigned to a role
groups        group      collection of groups assigned to a role
============  =========  =========================================================