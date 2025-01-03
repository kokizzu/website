---
title: presto
---

## Overview



{{< panel style="danger" >}}
Jsonnet source code is available at [github.com/grafana/jsonnet-libs](https://github.com/grafana/jsonnet-libs/tree/master/presto-mixin)
{{< /panel >}}

## Alerts

{{< panel style="warning" >}}
Complete list of pregenerated alerts is available [here](https://github.com/monitoring-mixins/website/blob/master/assets/presto/alerts.yaml).
{{< /panel >}}

### presto-alerts

##### PrestoHighInsufficientResources

{{< code lang="yaml" >}}
alert: PrestoHighInsufficientResources
annotations:
  description: The number of insufficient resource failures on {{$labels.instance}}
    is {{ printf "%.0f" $value }} which is greater than the threshold of 0.
  summary: The amount of failures that are occurring due to insufficient resources
    are scaling, causing saturation in the system.
expr: |
  increase(presto_QueryManager_InsufficientResourcesFailures_TotalCount[5m]) > 0
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### PrestoHighTaskFailuresWarning

{{< code lang="yaml" >}}
alert: PrestoHighTaskFailuresWarning
annotations:
  description: The number of task failures on {{$labels.instance}} is {{ printf "%.0f"
    $value }} which is above the threshold of 0.
  summary: The amount of tasks that are failing is increasing, this might affect query
    processing and could result in incomplete or incorrect results.
expr: |
  increase(presto_TaskManager_FailedTasks_TotalCount[5m]) > 0
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### PrestoHighTaskFailuresCritical

{{< code lang="yaml" >}}
alert: PrestoHighTaskFailuresCritical
annotations:
  description: The number of task failures on {{$labels.instance}} is {{ printf "%.0f"
    $value }} which is above the threshold of 30%s.
  summary: The amount of tasks that are failing has reached a critical level. This
    might affect query processing and could result in incomplete or incorrect results.
expr: |
  increase(presto_TaskManager_FailedTasks_TotalCount[5m]) / clamp_min(increase(presto_TaskManager_FailedTasks_TotalCount[10m]), 1) * 100 > 30
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### PrestoHighQueuedTaskCount

{{< code lang="yaml" >}}
alert: PrestoHighQueuedTaskCount
annotations:
  description: The number of queued tasks on {{$labels.instance}} is {{ printf "%.0f"
    $value }} which is greater than the threshold of 5
  summary: The amount of tasks that are being put in queue is increasing. A high number
    of queued tasks can lead to increased query latencies and degraded system performance.
expr: |
  increase(presto_QueryExecution_Executor_QueuedTaskCount[5m]) > 5
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### PrestoHighBlockedNodes

{{< code lang="yaml" >}}
alert: PrestoHighBlockedNodes
annotations:
  description: The number of blocked nodes on {{$labels.instance}} is {{ printf "%.0f"
    $value }} which is greater than the threshold of 0
  summary: The amount of nodes that are blocked due to memory restrictions is increasing.
    Blocked nodes can cause performance degradation and resource starvation.
expr: |
  increase(presto_ClusterMemoryPool_general_BlockedNodes[5m]) > 0
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### PrestoHighFailedQueriesWarning

{{< code lang="yaml" >}}
alert: PrestoHighFailedQueriesWarning
annotations:
  description: The number of failed queries on {{$labels.instance}} is {{ printf "%.0f"
    $value }} which is greater than the threshold of 0
  summary: The amount of queries failing is increasing. Failed queries can prevent
    users from accessing data, disrupt analytics processes, and might indicate underlying
    issues with the system or data.
expr: |
  increase(presto_QueryManager_FailedQueries_TotalCount[5m]) > 0
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### PrestoHighFailedQueriesCritical

{{< code lang="yaml" >}}
alert: PrestoHighFailedQueriesCritical
annotations:
  description: The number of failed queries on {{$labels.instance}} is {{ printf "%.0f"
    $value }} which is greater than the threshold of 30%s.
  summary: The amount of queries failing has increased to critical levels. Failed
    queries can prevent users from accessing data, disrupt analytics processes, and
    might indicate underlying issues with the system or data.
expr: |
  increase(presto_QueryManager_FailedQueries_TotalCount[5m]) / clamp_min(increase(presto_QueryManager_FailedQueries_TotalCount[10m]), 1) * 100 > 30
for: 5m
labels:
  severity: critical
{{< /code >}}
 
## Dashboards
Following dashboards are generated from mixins and hosted on github:


- [presto-coordinator](https://github.com/monitoring-mixins/website/blob/master/assets/presto/dashboards/presto-coordinator.json)
- [presto-logs](https://github.com/monitoring-mixins/website/blob/master/assets/presto/dashboards/presto-logs.json)
- [presto-overview](https://github.com/monitoring-mixins/website/blob/master/assets/presto/dashboards/presto-overview.json)
- [presto-worker](https://github.com/monitoring-mixins/website/blob/master/assets/presto/dashboards/presto-worker.json)
