=====
Event
=====


Events are designed for logging from your application and are stored in such a way
that large amounts of event entities can be stored and managed by the service.


Events are the primary way to store custom counter data for analytics.


.. _create-event:


-------------------
Create a New Event
-------------------


Create a new event in the "events" collection.


**Request URI**


 POST /{app_id}/events {request body}


**Parameters**


  :arg uuid|string app_id: application UUID or application name
  :request body: one or more sets of event properties.  You must provide a “timestamp” property but if you set it to 0, it will be assigned by the system:


  .. code-block:: js


    {
      "category" : "advertising",
      "counters" : {
    "ad_clicks" : 5
      }
    }




**Example - Request**


  .. code-block:: js


   POST http://api.usergrid.com/my-app/events {"category" : "advertising", "counters" :  {"ad_clicks" : 5}}
  
**Example - Response**


.. code-block:: js


  {
        "action": "post",
        "application": "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
        "params": {},
        "path": "/events",
        "uri": "http://localhost:8080/7fb8d891-477d-11e1-b2bd-22000a1c4e22/events",
        "entities": [
          {
            "uuid": "ce07ea3c-68b5-11e1-a586-9227e40e3559",
            "type": "event",
            "created": 1331166585282,
            "modified": 1331166585282,
            "counters": {
              "ad_clicks": 5
            },
            "metadata": {
              "path": "/events/ce07ea3c-68b5-11e1-a586-9227e40e3559"
            },
            "timestamp": 1331166585282
          }
        ],
        "timestamp": 1331166585018,
        "duration": 919
  }


You can create application-specific event properties in addition to these system-defined event properties: 
        
============  =========  =========================================================
Property      Type       Description
============  =========  =========================================================
uuid          UUID       the event's unique entity id
type          string     "event"
created       long       UNIX timestamp of entity creation
modified      long       UNIX timestamp of entity modification
timestamp     long       UNIX timestamp of application event (mandatory)
user          UUID       UUID of application user that posted the event 
group         UUID       UUID of application group that posted the event 
category      string     category used for organizing similar events
counters      map        counter used for tracking number of similar events
message       string     message describing event
============  =========  =========================================================


The counters property contains a set of name/value pairs to use for incrementing counter values.  For example::


  "counters" : {
    "ad_clicks" : 5
  }