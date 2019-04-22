# Monitoring a complex Azure solution

A strategy for monitoring a complex solution

## Problem Statement

Customer has a complex solution that includes App Services, serveless functions, VMs, and containers (k8s). They are not able to have a good monitoring strategy to monitor the application

## Objective

## Sample Solution

[Insert diagram]

## Solution

1) Create an application instance for each of the services in the solution including for the applications running in the VMs
2) Install the SDK and develop the custom events and telemetry
 - Out of the box Application Insights can track exceptions, dependency calls, traces, etc. 
3) Create an instance of Azure monitor 
4) Send all Application Insight data to Azure monitor
