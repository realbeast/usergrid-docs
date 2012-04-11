======
Folder
======


The Usergrid APIs can create, retrieve, update, delete and query folder entities. See the :ref:`gen-purpose-endpoints` section for descriptions of these APIs.


You can create application-specific folder properties in addition to these system-defined folder properties:
        
============  =========  =========================================================
Property      Type       Description
============  =========  =========================================================
uuid          UUID       the folder's unique entity id
type          string     "folder"
created       long       UNIX timestamp of entity creation
modified      long       UNIX timestamp of entity modification
owner         UUID       UUID of the folder’s owner (mandatory)
path          string     folder title (mandatory)
============  =========  =========================================================




Folders have the following set properties:


============  =========  =========================================================
Set           Type       Description
============  =========  =========================================================
connections   string     set of connections for the folder
============  =========  =========================================================