=====
Asset
=====


The Usergrid APIs can create, retrieve, update, delete and query asset entities. See the :ref:`gen-purpose-endpoints` section for descriptions of these APIs.


You can create application-specific asset properties in addition to these system-defined asset properties: 
        
============  =========  =========================================================
Property      Type       Description
============  =========  =========================================================
uuid          UUID       the asset's unique entity id
type          string     "asset"
created       long       UNIX timestamp of entity creation
modified      long       UNIX timestamp of entity modification
owner         UUID       UUID of the asset’s owner  
path          string     asset title
============  =========  =========================================================




Assets have the following set properties:


============  =========  =========================================================
Set           Type       Description
============  =========  =========================================================
connections   string     set of connections for the asset
============  =========  =========================================================