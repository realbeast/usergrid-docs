===========
Application
===========


Application entities are created via the Usergrid portal.  Once created, they can be accessed at:


  http://api.usergrid.com/<app_name_or_uuid>


Most mobile apps will never access the application entity directly.  You might have a server-side web app that
accesses the application entity for configuration purposes.  If you’re looking to access your application
entity programmatically, a good starting point is to view the source in the Usergrid portal.


You can create application-specific entity properties in addition to these system-defined application properties: 
        
=====================================   =========  =========================================================
Property                                Type       Description
=====================================   =========  =========================================================
uuid                                    UUID       the application's unique entity id
type                                    string     "application"
created                                 long       UNIX timestamp of entity creation
modified                                long       UNIX timestamp of entity modification
name                                    string     application name (mandatory)
title                                   string     application title
description                             string     application description 
activated                               boolean    whether application has been activated
disabled                                boolean    whether application has been administratively disabled
allowOpenRegistration                   boolean    whether application allows any user to register
registrationRequiresEmailConfirmation   boolean    whether registration requires email confirmation 
registrationRequiresAdminApproval       boolean    whether registration requires Admin approval
=====================================   =========  =========================================================


Look-up properties for the entities of type “application” are UUID and name. 


Applications have the following set properties:


==============   =========  ===============================================================
Set              Type       Description
==============   =========  ===============================================================
collections      string     set of collections  
rolenames        string     set of roles assigned to an application
counters         string     set of counters assigned to an application
oauthproviders   string     set of OAuth providers for the application
credentials      string     set of credentials required to run the application
webhooks         string     set of WebHooks (HTTP callbacks; see http://wiki.webhooks.org)
==============   =========  ===============================================================




Applications have the following collections:


============  =========  =========================================================
Collection    Type       Description
============  =========  =========================================================
users         user       collection of users 
groups        group      collection of groups 
folders       folder     collection of assets that represent folder-like objects
events        event      collection of events posted by the application
assets        asset      collection of assets that represent file-like objects
activities    activity   collection of activity stream actions
devices       device     collection of devices in the service  
============  =========  =========================================================