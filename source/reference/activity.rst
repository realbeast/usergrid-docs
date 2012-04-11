========
Activity
========


Activities are an entity type for representing activity stream actions (see the 
`JSON Activity Streams 1.0 specification <http://activitystrea.ms/specs/json/1.0/>`_).


The Usergrid APIs can create, retrieve, update, delete and query activity entities. See the :ref:`gen-purpose-endpoints` section 
for descriptions of these APIs. 


You can create application-specific activity properties in addition to these system-defined activity properties:
        
============  ==============   =========================================================================================
Property      Type             Description
============  ==============   =========================================================================================
uuid          UUID             the activity's unique entity id
type          string           "activity"
created       long             UNIX timestamp of entity creation
modified      long             UNIX timestamp of entity modification
actor         ActivityObject   entity that performs the action of the activity (see `JSON Activity Streams 1.0 specification <http://activitystrea.ms/specs/json/1.0/>`_)
content       string           description of the activity
icon          MediaLink        visual representation of a Media Link resource (see `JSON Activity Streams 1.0 specification <http://activitystrea.ms/specs/json/1.0/>`_)
category      string           category used to organize activities
verb          string           action that the actor performs (e.g., “likes”, “follows”, etc.) 
published     long             UNIX timestamp when the activity was published
object        ActivityObject   object on which the action is performed (see `JSON Activity Streams 1.0 specification <http://activitystrea.ms/specs/json/1.0/>`_)
title         string           title or headline for the activity 
============  ==============   =========================================================================================




Activities have the following set properties:


============  =========  =========================================================
Set           Type       Description
============  =========  =========================================================
connections   string     set of connections for the activity
============  =========  =========================================================