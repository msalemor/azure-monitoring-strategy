# Monitoring a complex Azure solution

A strategy for monitoring a complex solution

## Problem Statement

Customer has a complex solution that includes App Services deployed to several regions, PaaS Services, serveless functions, VMs, and containers (k8s). They are having problems monitoring the solution comprehensively.

## Objective

Customer wishes to deploy a comprenhesive monitoring strategy to be able to quickly detect problems and raise alerts individually and accross the solution.

## Sample Solution

[Insert diagram]

## Solution

1) Create an application instance for each of the services in the solution including for the applications running in the VMs
2) Install the SDK and develop the custom events and telemetry
 - Out of the box Application Insights can track exceptions, dependency calls, traces, etc.
 - Additional telemetry can be developed using the SDK
3) Create an instance of Azure monitor 
4) Send all Application Insight data to Azure monitor
