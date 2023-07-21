# Executive Dashboard

This dashboard consists of the following widgets:
| Widget | Sample |
| ------ | ------ |
|<b>TOTAL DEPLOYMENTS</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `deploymentId`| <img src=img/TotalDeployments.png width="300"> |
|<b>DEV</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `deploymentId` <br>Breakdown by `filter`: `(comandContextFilter.keyword : *dev*)`| <img src=img/Dev.png width="300"> |
|<b>TEST</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `deploymentId` <br>Breakdown by `filter`: `(comandContextFilter.keyword : *test*)`| <img src=img/Test.png width="300"> |
|<b>STAGE</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `deploymentId` <br>Breakdown by `filter`: `(comandContextFilter.keyword : *stage*)`| <img src=img/Stage.png width="300"> |
|<b>PROD</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `deploymentId` <br>Breakdown by `filter`: `(comandContextFilter.keyword : *prod*)`| <img src=img/Prod.png width="300"> |
|<b>APPLICATION COUNT</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `commandLabelFilter.keyword` <p>Note: Use [custom log data](https://docs.liquibase.com/tools-integrations/observability/meta-data-structured-logs.html) instead of `commandLabelFilter` | <img src=img/ApplicationCount.png width="300"> |
|<b>LOB1 (Commercial)</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `commandLabelFilter.keyword`, Advanced: Filter by `commandLabelFilter.keyword : *Commercial*` <p>Note: Use [custom log data](https://docs.liquibase.com/tools-integrations/observability/meta-data-structured-logs.html) instead of `commandLabelFilter` | <img src=img/Commercial.png width="300"> |
|<b>LOB1 (Global)</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `commandLabelFilter.keyword`, Advanced: Filter by `commandLabelFilter.keyword : *Global*` <p>Note: Use [custom log data](https://docs.liquibase.com/tools-integrations/observability/meta-data-structured-logs.html) instead of `commandLabelFilter` | <img src=img/Global.png width="300"> |
|<b>LOB1 (Consumer)</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `commandLabelFilter.keyword`, Advanced: Filter by `commandLabelFilter.keyword : *Consumer*` <p>Note: Use [custom log data](https://docs.liquibase.com/tools-integrations/observability/meta-data-structured-logs.html) instead of `commandLabelFilter` | <img src=img/Consumer.png width="300"> |
|<b>Database Endpoint Count</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `liquibaseTargetUrl.keyword`| <img src=img/DatabaseEndpointCount.png width="300"> |
|<b>Platform1 (MongoDB)</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `liquibaseTargetUrl.keyword` <br>Breakdown by `filter`: `liquibaseTargetUrl.keyword: *mongodb*`| <img src=img/MongoDB.png width="300"> |
|<b>Platform2 (Oracle)</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `liquibaseTargetUrl.keyword` <br>Breakdown by `filter`: `liquibaseTargetUrl.keyword: *oracle*`| <img src=img/Oracle.png width="300"> |
|<b>Platform3 (Snowflake)</b><br>Visualization: <b>Metric</b><br> Primary metric: `Unique count` of `liquibaseTargetUrl.keyword` <br>Breakdown by `filter`: `liquibaseTargetUrl.keyword: *snowflake*`| <img src=img/Snowflake.png width="300"> |
|<b>Success/Failure Rate (Aggregate)</b><br>Visualization: <b>Donut</b><br>Slice by `Top values` of `deploymentOutcome.keyword`, top 2 values <br>Metric: `count`: `deploymentOutcome.keyword` | <img src=img/SuccessFailureRate-pie.png width="300"> |
|<b>Success/Failure Rate </b><br>Visualization: <b>Area</b><br>Horizontal Axis: `Date histogram`: `timestamp` <br>Vertical axis: `Count`: `deploymentOutcome.keyword` <br>Breakdown: `Top values`:`deploymentOutcome.keyword`, Top 2 values | <img src=img/SuccessFailureRate-area.png width="300"> |
|<b>Releases by Top Apps </b><br>Visualization: <b>Donut</b><br>Slice by: `Top values`: `commandLabelFilter.keyword`, Number of values: 9, Rank by `Count of commandLabelFilter.keyword` <br>Metric: `Count`: `commandLabelFilter.keyword` <p>Note: Use [custom log data](https://docs.liquibase.com/tools-integrations/observability/meta-data-structured-logs.html) instead of `commandLabelFilter` | <img src=img/ReleasesByTopApps.png width="300"> |
|<b>Average Lead Time to Change 1 </b><br>Visualization: <b>Metric</b><br>Primary Metric: Formula: `(max(changesetOperationStart, kql = 'commandContextFilter.keyword:prod')-min(changesetOperationStop, kql = 'commandContextFilter.keyword:dev'))/36000/count(changesetId.keyword)` <br>Breakdown by `filters`: <p>`commandLabelFilter.keyword : *Commercial*` <br>`commandLabelFilter.keyword : *Global*` <br>`commandLabelFilter.keyword : *Wealth*` <br>`commandLabelFilter.keyword : *Consumer*` <br>`commandLabelFilter.keyword : *Risk*` <br>`commandLabelFilter.keyword : *Management*` <br>Appearance: Layout columns: `3` <p>Note: Use [custom log data](https://docs.liquibase.com/tools-integrations/observability/meta-data-structured-logs.html) instead of `commandLabelFilter` | <img src=img/AverageLeadTimeToChange1.png width="300"> |
|<b>Average Lead Time to Change 2 </b><br>Visualization: <b>Metric</b><br>Primary Metric: Formula: `(max(changesetOperationStart, kql = 'commandContextFilter.keyword:prod')-min(changesetOperationStop, kql = 'commandContextFilter.keyword:dev'))/36000/count(changesetId.keyword)` <br>Breakdown by: `Top values` of `commandLabelFilter.keyword` <br>Appearance: Layout columns: `3` <p>Note: Use [custom log data](https://docs.liquibase.com/tools-integrations/observability/meta-data-structured-logs.html) instead of `commandLabelFilter` | <img src=img/AverageLeadTimeToChange2.png width="300"> |