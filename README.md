# LIQUIBASE DASHBOARDS 

This repo will host information about common dashboards that can be built using Liquibase Pro's structured logging output. 

We will provide sample dashboards for AWS Cloudwatch with Grafana, Datadog, Splunk and Elastic

Common metrics:

* Deployment Frequency
* Change Failure Rate
* Lead Time To Change
* MTTR (Mean Time To Recovery)

## Deployment Frequency

Frequency of database deployments 
## Change Failure Rate

Percentage of deployment that fail

## Lead Time To Change

Time between when a change was first seen in the pipeline (e.g., first deployment to DEV) and the time it was successfully deployed to PROD
## MTTR (Mean Time To Recovery)

Time between when a deployment fails in PROD and the time it take for the next successful deployment in PROD.

