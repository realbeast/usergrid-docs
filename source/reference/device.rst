==========
Device
==========


The Usergrid APIs can create, retrieve, update, delete and query device entities. See the :ref:`gen-purpose-endpoints` section for descriptions of these APIs.


You can create application-specific device properties in addition to these system-defined device properties:
        
============  =========  ===========================================================
Property      Type       Description
============  =========  ===========================================================
uuid          UUID       entity unique id
type          string     entity type (e.g., "device")
created       long       UNIX timestamp of entity creation
modified      long       UNIX timestamp of entity modification
name          string     device name (mandatory)
============  =========  ===========================================================


Look-up properties for the entities of type “device” are UUID and name. 




Users have the following associated collections:


============  =========  =========================================================
Collection    Type       Description
============  =========  =========================================================
users         user       collection of users to which a device belongs
============  =========  =========================================================