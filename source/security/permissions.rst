.. _permissions-section:


=====================
Roles and Permissions
=====================


When a Usergrid API request is made and no organization-level or application-level client
credentials are provided, user-level permissions determine
what operations are allowed. Usergrid checks permissions assigned to
the user and the user’s roles against the resource paths that the user is trying to access.


-----
Roles
-----


Every application has two roles by default:


* Guest - the guest role for unauthenticated users
* Default - the default role for authenticated users


The Guest role has a basic set of permissions for being able to create a new user
or to register a device. These permissions can be removed and/or new ones can be
added. The Default role represents the minimal set of operations you want an authenticated user
to be able to perform.


-----------
Permissions
-----------


To specify permissions, the format is as follows::


  <operations>:<entity path pattern>


The <operations> field is a comma-delimited set of REST operations
(GET, PUT, POST, DELETE) that are allowed for the specified entity path. 
The <entity path pattern> parameter is evaluated using Apache Ant pattern matching
(see http://ant.apache.org/manual/dirtasks.html#patterns).


When you select Roles from the sidebar of the Developer Portal and click on a particular role, 
specific privileges for operations are represented using checkboxes. Behind the scenes, Usergrid saves permission settings as a comma-separated list of operations and path patterns.


Permission Examples
-------------------


Here are some examples to illustrate how permissions are specified:


* A permission that allows anyone to create a user::


    post:/users/*


* A permission that allows someone to look at a specific user::


    get:/users/edanuff


* A permission that allows the current user to see their activity feed::


    get:/users/${user}/feed/*


  The "${user}" in the entity path refers to a variable that represents the current user's UUID.


* A permission allowing linked entities to be read::


    get:/users/${user}/**


  This specification matches multiple resource paths, including, but not limited to, the following::


    /users/${user}/feed
    /users/${user}/feed/item1/a/b/c