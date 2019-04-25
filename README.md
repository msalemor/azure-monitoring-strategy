# Monitoring a complex Azure solution

A strategy for monitoring a complex solution

## Problem Statement

Customer has a complex solution that includes App Services deployed to several regions, PaaS Services, serveless functions, VMs, and containers (k8s). Currently they don't have a comprenhensive strategy to monitor the individual pieces of the solution and comprenhensively.

## Objective

Customer wishes to deploy a comprenhesive monitoring strategy to be able to quickly detect problems and raise alerts individually and accross the solution.

## Solution approaches

In order to achieve a comprehensive monitoring apprach, customer is evaluates having one or many Log Analytics instances.

## Solution diagram (from a monitoring perspective)

![Complex solution](https://github.com/msalemor/azuremonitoring/blob/master/Complex%20Solution.png)

### Log Analytics

Create a log analytics instance for the solution (i.e. **Complex Solution - Analytics**)

### Azure Monitor

For the items in organge, turn on diagnostics logs using Azure Monitor and send the logs to the Log Analytics instance

### App Insights

For the items in red, create an Application Instance instance for each of the services.

<!--
## Solution

1) Create an Application Insights instance for each of the services in the solution including for the applications running in the VMs
2) Install the SDK and develop the custom events and telemetry
 - Out of the box Application Insights can track exceptions, dependency calls, traces, etc. (about 85% of the useful information)
 - Additional telemetry can be developed using the SDK. This is the recommended practice to make the most of the solution.
   - For example, suppose the application is able to detect that responses from the server are taking longer than they should. The application then could raise a custom event indicating this condition.
3) Create an instance of Azure monitor 
4) Send all Application Insight data to Azure monitor
-->
## Monitoring and alerting strategy

<!--
| Event | Severity | Determination | Action |
| --- | --- | --- | --- |
| Entire region goes down | Critical | All services from a single region not reporting | Activate DR strategy <br/> - Step 1 |
| Failure in infrastructure services | Critical | |
| Failure in SaaS Services | High | |
| Failure in IaaS Services | High | |


| Event | Determination and Action |
| --- | --- |
| Entire region goes down | Determination: <br/> No services reporting from one region <br/> Actions: <ul><li></li></ul> |
-->

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

