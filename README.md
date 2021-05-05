# Monitoring A Complex Azure Solution

## Problem Statement

Customer has a complex solution that includes App Services deployed to several regions, PaaS Services, serveless functions, VMs, and containers (k8s). Currently they don't have a comprenhensive strategy to monitor the individual pieces of the solution and comprenhensively.

## Objective

Customer wishes to deploy a comprenhesive monitoring strategy to be able to quickly detect problems and raise alerts individually and accross the solution.

## Glossary of Terms

- **Activity Log:** Management plane operations (who, what, when). Information includes operation performed, timestamp, user or service performing the operation). Logs by default are stored for 90 days.
- **Diagnostic Log:** Resource level logs providing information about the specific resource.
- **Metric:** Point in time measurement (cpu utilitization, memory usage, etc.). Default is one-minute frequence and are automatically available. Default retantion is 93 days.
- **Retention Period:**
- **Log Analytics Workspace:** A data repository where all logs and metrics are stored.
  - **Kusto Query Languange (KQL):** Language used to query Log Analytics
- **Diagnostics settings:** Location where logs and metrics can be sent including Log Analytics, Event Hubs, Storage Accounts.

## Sample Solution diagram (from a monitoring perspective)

![Complex solution](https://github.com/msalemor/azuremonitoring/blob/master/Complex%20Solution.png)

For the solution above, some activites could include:
- Creating a Log Analytics instance for the solution
- For dependent infrastructure and PaaS services (the items in organge), turn on diagnostics logs and metrics using Azure Monitor and send the logs to the Log Analytics instance
- For the applications (items in red), create an Application Insight instance for each of the applications.

Critical events to monitor for:

| Event | Severity | Determination | Action |
| --- | --- | --- | --- |
| Entire region goes down | Critical | All services from a single region not reporting | Activate DR strategy <br/> - Step 1 |
| Dependent infrastructure failure | Critical | For example, not network connectivity | Open a suppor ticket and activate DR strategy |
| Dependent PaaS, IaaS, SaaS failure | Critical | For example, Azure SQL Server failure | open a support ticket |
| Dependent Application failure | Critical | For example, an on-prem service is not responding | Review logs and escalate issue |
| Performance degradation | High | For example, cache store is not responding | Review logs and open a support ticklet |

## Comprehensive monitoring strategy

- Understand the fundamentals such as:
  - Why do we monitor?
  - the difference between activity and diagnostic logs and metrics, 
  - leveraging Azure Monitor and Application Insights, 
  - Log Analytics.
- Quickly identify security, performance, and availability issues by analyzing the telemetry, activity logs and diagnostic data to be able to restore unhealthy or underperforming systems to healthy state.

## Monitoring Fundamentals

### Why do we monitor?

- Is the service healthy?
- Is the service available (different locations)?
- Is the service secure?
- Is the service performing as expected?
- Is the server adhering to compliance?
- What does a service depend upon?

## Azure Monitor

- Single source for monitoring Azure Resources
- Gives insights into metrics
- Activity and Diagnostics Logs
- Alerts from different sources including Azure Monitor, Log Analytics and Application Insights
- Fast metric pipelines enabling time critical alerts and notifications

##  Activity Logs

Provides insights into all operations performed on resources within a subscription

- Operation performed
- Time
- User or service performing the operation
- JSON can be view of exactly what change occurred
- Queries can be created and saved/pinned
- Logs are stored for 90 days. 
  -	For longer retention send it to Log Analytics

## Diagnostics Logs

- Resource-level logs providing insight into operations within the specific resource. These can be exported to:
  - Azure Storage
  - Log Analytics Workspace
  - EventHubs (integration with Splunk, etc.)
  - Configured centrally in Azure Monitor (or on each resource via its Diagnostic logs settings)
  
> Links:<br/>- [Diagnostic logs schema](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-logs-schema)


## Metrics

- Provides visibility into performance and health of workloads via metrics (performance counters) emitted by most resources
- Default of one-minute frequency are automatically available
- Azure Monitor provides a central location to view metrics for all resources within the subscription

Links:
- [Metrics Supported](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/metrics-supported)

## Dashboards

- Supports multiple dashboards
- Multiple Charts can be added to dashboards
- Dashboards can be download and uploaded in JSON format
- Dashboards can be shared with access control to restrict who can utilize them

Links:
- [Azure Portal Dashboards](https://docs.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards)

### Status.azure.com

- Allows you to view the state of resources across all Azure regions and allows you to create a customized view of the state of these resources and regions you care about.

### Dashboard - Guidance and Recommendations

- Building the graphs will help with identify the thresholds that need to be alerted on.

## Action Groups

- Action be used by multiple alerts
- Action group is made up of one or more actions (Name, action type, details of the action)
- Action types can trigger automated actions, contact groups and integrate with ITSM systems

### Action Groups - Guidance and Recommendations

- Setup at least on action group to manage alert

## Alerts

- Choose a scope (subscription, resource group, resource, region)
- Create a condition
- Select or create an action
- Name the alert and set the severity

### Alerts - Guidance and Recommendations

- Create meaningful alerts

## Application Insights

- Application insights constantly monitors the application for performance and problems
- Can perform analytics (with Azure monitor) to understand anomalies and then alert on those
- Baselines are established and if the baseline is stepped outside above or below then notified.
- Monitor for application can be done from within Visual Studio or from the portal
- Alerts can be created to proactively notify to enable resolution before customer s impacted with granular detail of the cause

### Application Insights - Guidance and Recommendations

- Deploy an Application Insight instance per application per region
- Add the SDK to the application to take advantage of custom telemetry and custom events
- You can perform aggregated queries from Log Analytics to monitor and alert across applications and tiers:

```union withsource= SourceApp
app('Contoso-app1').requests, 
app('Contoso-app2').requests,
app('Contoso-app3').requests,
app('Contoso-app4').requests,
app('Contoso-app5').requests
```

Links:

- [Cross workspace query](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/cross-workspace-query)

## Log Analytics

- Log analytics ingests logs from many sources and in different formats including metrics typically in Azure Monitor
- A workspace is an instance of Log Analytics and multiple workspaces can exist within a subscription
- Queries are executed against the logs to produce insights in the state of systems
- Manage solutions build on logs and queries to offer service-based insight, for example AD health, path status and SQL best practices.
- Data can be retained for a configured period

Links:
- [Manage access](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-access)

### Log Analytics - Guidance and Recommendations

- One instance per region
- By mindful of retention and cost

<!--
A strategy for monitoring a complex solution

### Log Analytics

Create a log analytics instance for the solution (i.e. **Complex Solution - Analytics**)

### Azure Monitor

For the items in organge, turn on diagnostics logs using Azure Monitor and send the logs to the Log Analytics instance

### App Insights

For the items in red, create an Application Instance instance for each of the services.


## Solution

1) Create an Application Insights instance for each of the services in the solution including for the applications running in the VMs
2) Install the SDK and develop the custom events and telemetry
 - Out of the box Application Insights can track exceptions, dependency calls, traces, etc. (about 85% of the useful information)
 - Additional telemetry can be developed using the SDK. This is the recommended practice to make the most of the solution.
   - For example, suppose the application is able to detect that responses from the server are taking longer than they should. The application then could raise a custom event indicating this condition.
3) Create an instance of Azure monitor 
4) Send all Application Insight data to Azure monitor

## Monitoring and alerting strategy



| Event | Determination and Action |
| --- | --- |
| Entire region goes down | Determination: <br/> No services reporting from one region <br/> Actions: <ul><li></li></ul> |

- Entire region goes down
  - All infrastructure services not reporting
  - Frontend and APIs not reporting
  - DR strategy shoulkd kickoff including:
    - All read and writes should take place from the secondary region
- Failure in depended infrastructure services including VNet, Application Gateway, Traffic Manager, etc.)
- Failure in frontend
- Failure in the dependent services (service running as APIs, kubernetes, functions, databases, etc.)
  - Note that a failure in Redis cache may not stop the application from working, but it may prevent the application from performing at maximum performance.
- More resources are needed due to an increase in demand

-->
