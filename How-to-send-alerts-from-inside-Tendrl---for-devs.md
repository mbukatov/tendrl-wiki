# Supporting a new type of alert in tendrl

## How alerting works

* The node-agent exposes a unix socket for the purpose of logging.
  Any message(job-updates, threshold breaches, status changes, exception tracebacks etc...) posted on this socket by 
  any of the tendrl's integrations, with appropriate meta-data lands into a designated directory in etcd -- 
  '/messages/events' and this happens after many validations and in accordance with different priorities attached to 
  the message which is all handled by the logging framework in commons..
* Now there is a specific designated priority value('notice') to consider these events in etcd as of importance to be 
  notified to the user and the alerting module is specifically only interested in messages with this priority present 
  in etcd and provided the message contains a specific set of meta-data.
* The alerting module periodically scans through entries in /messages/events looking for any new message in this 
  directory in etcd that carries a 'notice' priority. If there's any, it hands over such a message to the alerting 
  module's handler framework that chooses the handler specific to the message(alert) and does some processing around 
  it like the following

  * Classify the alert as node related or cluster related...
  * Process any meta-data attached to the raw-message in etcd.
* After all of this the alert now is pushed into the notification framework of the alerting module. Now the different 
  means of notification in this framework are hooked in the form of plugins and the framework just handovers the alert 
  to each of these plugins which in turn decide their destinations for the alert based on the specific configurations 
  they work on..

## How can a new alert be added/supported by tendrl

* The alert needs to contain the following fields:
---
    Alert:
      attrs:
        alert_id:
          help: 'The unique identifier of alert'
          type: String
        node_id:
          help: 'The unique identifier of node on which alert was detected'
          type: String
        time_stamp:
          help: 'The timestamp at which alert was observed'
          type: String
        resource:
          help: 'The resource with problem for which alert was raised'
          type: String
        current_value:
          help: 'The current magnitude(status/utilization) of problem'
          type: String
        tags:
          help: 'Alert specific fields that cannot be generalized for all alerts like the cluster-id, cluster-name, descriptive message detailing the problem'
          type: Dict
        alert_type:
          help: 'The type(status/percentage utilization) of alert'
          type: String
        severity:
          help: 'The severity of alert'
          type: String
        significance:
          help: 'The significance of notifying alert'
          type: String
        ackedby:
          help: 'Entity/person acking the alert'
          type: String
        acked:
          help: 'Indication of whether alert is acked or not'
          type: Boolean
        ack_comment:
          help: 'Users comments for acking this alert'
          type: List
        acked_at:
          help: 'Time at which the alert was acked'
          type: String
        pid:
          help: 'The id of process raising the alert'
          type: String
        source:
          help: 'The process raising the alert'
          type: String
      enabled: true
      value: alerting/alerts/$Alert.alert_id
      list: alerting/alerts
      help: "alerts"
---

* Now any tendrl integration to add an alert related to the change in status/availability of an entity/resource 
  maintained by it, needs to pass such an information encoded with details in the above mentioned format to the node-
  agent logging socket using the Message-Event framework exposed by the commons logging framework with a priority 
  'notice'.
---
    ex: https://github.com/Tendrl/node-monitoring/blob/develop/tendrl/node_monitoring/plugins/handle_collectd_notification.py#L117
---

* Once all this is done, a alert specific handler needs to be added in the alerting module.Currently the role of the 
  handlers is minimal but they are expected to carry more responsibilities once their requirements are felt.
---
    ex:Handlers under https://github.com/Tendrl/alerting/tree/develop/tendrl/alerting/handlers
---
