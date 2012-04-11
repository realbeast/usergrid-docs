=========================
Authentication and Access
=========================


Usergrid requests are authenticated via OAuth (Open Authorization) 2.0. OAuth is an authentication 
mechanism that allows users to grant access to their web resources or mobile apps safely,
without having to share their passwords. The analogy of a valet key is sometimes used to describe 
OAuth since users can permit access but limit access rights to perform certain operations. Instead of app 
user having to share a password, OAuth enables access using a security token that’s tied specifically to an app and device.


Unlike OAuth 1.0, which requires special support in the client code for signing requests, 
OAuth 2.0 can be used by any web service client libraries. Although the OAuth 2.0 
specification isn’t finalized yet, it's sufficiently complete such that many web service 
providers, including Google and Facebook, have switched to using it for authentication. 
More information about OAuth 2.0 is available at `oauth.net <http://oauth.net/2/>`_.


------------
Access Types
------------


Usergrid takes advantage of standard OAuth 2.0 mechanisms that require an access token with 
data operation requests. To obtain the access token, you first connect to an appropriate web 
service endpoint and provide the correct client credentials. The credentials you must provide to get 
the token depend on the type of access required for the operation. 


There are four access types:


=================  ==============================================================================================================
Access Type        Description
=================  ==============================================================================================================
Organization       Full access to perform any operation on a Usergrid Organization account
Admin User         Full access to perform any operation on all Organization accounts of which the Admin User is a member 
Application        Full access to perform any operation on a Usergrid Application
Application User   Policy-limited access to perform operations on a Usergrid Application
=================  ==============================================================================================================


Organization or Application access are intended for server-side applications since they 
are "superuser" access mechanisms that have few constraints on what they are permitted to do. 
When connecting via OAuth, you supply Organization or Application client_id and client_secret parameters as client credentials. 
The Home page of the Developer Portal shows these parameters for the Organization, while the Settings page 
displays them for the currently selected app.


Admin User and Application User access are appropriate for connections on behalf of a known user account. This could be 
either an Admin User who is a member of several organizations and has management rights to several apps, or
an Application User authorized for an application. When connecting via OAuth on behalf of a known user,
you supply username and password parameters as client credentials.


Each access type is discussed below and shows how to supply an access token in an API request.




Organizations
-------------


When you sign up for the Usergrid Developer Portal, you create an Organization account in addition
to your personal Admin User account. Access to a single Organization requires client_id/client_secret credentials 
(or an Admin User can supply username/password credentials, as shown in the next section).


As an example of accessing an Organization, you can obtain your client_id/client_secret values from the Developer Portal and connect to 
the following URL (substituting the correct values for <client_id> and <client_secret>)::


  http://api.usergrid.com/management/management/token?grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>


The results look something like this::


    {
      "access_token": "gAuFEOlXEeCfRgBQVsAACA:b3U6AAABMqz-Cn0wtDxxkxmQLgZvTMubcP20FulCZQ",
      "expires_in": 3600,
      "organization": {
        "users": {
          "test": {
            "name": "Test User",
            "disabled": false,
            "uuid": "7ff1946d-e957-11e0-9f46-005056c00008",
            "activated": true,
            "username": "test",
            "applicationId": "00000000-0000-0000-0000-000000000001",
            "email": "test@usergrid.com",
            "adminUser": true,
            "mailTo": "Test User <test@usergrid.com>"
          }
        },
        "name": "test-organization",
        "applications": {
          "test-app": "8041893b-e957-11e0-9f46-005056c00008"
        },
        "uuid": "800b8510-e957-11e0-9f46-005056c00008"
      }
    }


The results show the access_token needed to make subsequent API requests
as well as additional information about the organization.




Admin Users
-----------


Admin Users are users of the Usergrid.com service as well as members of one or more Organizations. 
An Organization, in turn, can have one or more Admin Users. Currently all Admin Users in an 
Organization have full access permissions once they authenticate using basic username/password 
credentials. At a future point in time, Usergrid may support a more fine-grained, delegated administration model 
in which access rights for Admin Users are configurable.  


As an example of authenticating as an Admin User, use the username/password values specified when you created your Admin User account and connect to the following URL (substituting the correct values for <username> and <password>)::


  http://api.usergrid.com/management/management/token?grant_type=password&username=<username>&password=<password>


The results look something like this::


    {
      "access_token": "f_GUbelXEeCfRgBQVsAACA:YWQ6AAABMqz_xUyYeErOkKjnzN7YQXXlpgmL69fvaA",
      "expires_in": 3600,
      "user": {
        "username": "test",
        "email": "test@usergrid.com",
        "organizations": {
          "test-organization": {
            "users": {
              "test": {
                "name": "Test User",
                "disabled": false,
                "uuid": "7ff1946d-e957-11e0-9f46-005056c00008",
                "activated": true,
                "username": "test",
                "applicationId": "00000000-0000-0000-0000-000000000001",
                "email": "test@usergrid.com",
                "adminUser": true,
                "mailTo": "Test User <test@usergrid.com>"
              }
            },
            "name": "test-organization",
            "applications": {
              "test-app": "8041893b-e957-11e0-9f46-005056c00008"
            },
            "uuid": "800b8510-e957-11e0-9f46-005056c00008"
          }
        },
        "adminUser": true,
        "activated": true,
        "name": "Test User",
        "mailTo": "Test User <test@usergrid.com>",
        "applicationId": "00000000-0000-0000-0000-000000000001",
        "uuid": "7ff1946d-e957-11e0-9f46-005056c00008",
        "disabled": false
      }
    }


Note the access_token needed to make subsequent API requests on behalf of the Admin User.


------------
Applications
------------


Applications can be accessed in three ways:


* With Application client_id/client_secret credentials 
* With the client_id/client_secret credentials of the Organization that owns the application
* With username/password credentials of an Admin User associated with the application’s Organization


Using your client_id/client_secret values (obtained from the Application
Settings section of the Developer Portal), you can connect to the following URL
(substituting the correct values for <app-name>, <client_id>, and
<client_secret>)::


  http://api.usergrid.com/<app-name>/token?grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>


The results look something like this::


    {
      "access_token": "F8zeMOlcEeCUBwBQVsAACA:YXA6AAABMq0d4Mep_UgbZA0-sOJRe5yWlkq7JrDCkA",
      "expires_in": 3600,
      "application": {
        "name": "test-app",
        "id": "17ccde30-e95c-11e0-9407-005056c00008"
      }
    }


The results show the access_token needed to make subsequent API requests on behalf of the Application.


-----------------
Application Users
-----------------


Application Users are members of the "users" collection within an 
application. They are the actual users of an app and their data is stored separately 
from any other app and from the Usergrid.com service. 


Application Users can authenticate with either basic username/password credentials or 
OAuth client_id/client_secret credentials. Once authenticated, these users can access
Usergrid entities depending on their assigned permissions, their roles, and the 
permissions assigned to those roles.


Using the username/password values specified when the Application User was
created, you can connect to the following URL (substituting the correct values
for <app-name>, <username>, and <password>)::


  http://api.usergrid.com/management/<app-name>/token?grant_type=password&username=<username>&password=<password>


The results look something like this::


    {
      "access_token": "5wuGd-lcEeCUBwBQVsAACA:F8zeMOlcEeCUBwBQVsAACA:YXU6AAABMq0hdy4Lh0ewmmnOWOR-DaepCrpWx9oPmw",
      "expires_in": 3600,
      "user": {
        "uuid": "e70b8677-e95c-11e0-9407-005056c00008",
        "type": "user",
        "username": "edanuff",
        "email": "ed@anuff.com",
        "activated": true,
        "created": 1317164604367013,
        "modified": 1317164604367013
      }
    }


The results show the access_token needed to make subsequent API requests on behalf of the Application User.


----------------------
Using An Access Token
----------------------


Once you obtain an access_token, you must provide it with every subsequent
API call that you make. It can either be added to the API querystring, as in::


  http://api.usergrid.com/<app-name>/users?access_token=<access_token>


Or, you can provide it in an HTTP authorization header in the form of::


  Authorization: Bearer <access_token>


Please note that the Usergrid documentation assumes you are providing a valid 
access_token with every API call whether or not it is shown explicitly in the examples. Unless
the documentation specifically says that an API endpoint can be accessed
without an access_token, you should assume that you must provide it.


------------------
Safe Mobile Access   
------------------


For mobile access, it is recommended that you connect as an Application User with 
configured access control policies. Mobile applications are inherently
untrusted since they can be easily examined and even decompiled. 


Any credentials stored in a mobile app should be considered secure only to the level of the
Application User. This means that if you don't want the user to be able to
access or delete data in your Usergrid application, you need to make
sure that you don't enable that capability via roles or permissions. Since most web applications talk to
the database with root or some elevated level of permissions, it's generally a good idea for 
mobile applications to connect with a more restricted set of permissions. For more information, 
see the discussion of :ref:`permissions-section` in the next section.