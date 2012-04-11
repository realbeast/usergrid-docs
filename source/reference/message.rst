==========
Message
==========


The Usergrid APIs can create, retrieve, update, delete and query message entities. See the :ref:`gen-purpose-endpoints` section for descriptions of these APIs. 


You can create application-specific message properties in addition to these system-defined message properties:
        
==============  =========  ============================================================
Property        Type       Description
==============  =========  ============================================================
uuid            UUID       entity unique id
type            string     entity type (e.g., "message")
created         long       UNIX timestamp of entity creation
modified        long       UNIX timestamp of entity modification
correlation_id  string     an application-specific corrleation string 
destination     string     an application-specific destination string 
reply_to        string     application-specific destination where reply should be sent
category        string     category used for organizing messages (mandatory)
indexed         boolean    whether to index the message
persistent      boolean    whether to store the message
counters        map        set of counter names mapped to increment/decrement values
==============  =========  ============================================================


The look-up property for entities of type “message” is the UUID. 


The counters property contains a set of name/value pairs to use for incrementing counter values.  For example::


 "counters" : {
   "ad_clicks" : 5
 }