---
lab:
    title: 'Lab 11: Implement Monitoring'
    module: 'Administer Monitoring'
---

# Lab 11 - Implement Monitoring

## Lab introduction

In this task, I learned about Azure Monitor. I also configured alerts and checked the 
activity log for more insights.

## Lab scenario

Your organization has migrated their infrastructure to Azure. It is important that Administrators are notified of any significant infrastructure changes. You plan to examine the capabilities of Azure Monitor, including Log Analytics.

![Diagram of the architecture lab 11](../media/az104-lab11-architecture.png)

## Tasks:

+ Task 1: Use a template to provision an infrastructure.
+ Task 2: Create an alert.
+ Task 3: Configure action group notifications.
+ Task 4: Trigger an alert and confirm it is working.
+ Task 5: Configure an alert processing rule.
+ Task 6: Use Azure Monitor log queries.


### Using a template to provision an infrastructure.
In this task, I deployed a custom template that was provide by Microsoft.
After deployment of a template, I used Azure Monitor to Enable Insights for the Virtual Machine.
![Screenshot of the Monitor insights](../media/az104-lab11-architecture.png)

### Creating an alert
In this task, I configured an alert using Azure Monitor.
![Screenshot of the Monitor Alert](../media/az104-lab11-architecture.png)

### Configuring action group notifications
In this task, I configured an Action group and used my email to test if this alert will work.
![Screenshot of the Action Group](../media/az104-lab11-architecture.png)

### Triggering an alert and confirm it is working.
In this task, I deleted a VM to test if this alert will work.
While I was waiting for an email, I checked the logs to see if the VM is deleted.
![Screenshot of the Monitor Alert](../media/az104-lab11-architecture.png)

### Configuring an alert processing rule
In this task, I created an alert rule to suppress notifications during a maintenance period.
![Screenshot of the alert processing rule](../media/az104-lab11-architecture.png)

### Using Azure Monitor log queries
In this task, in Azure Monitor I used a few different queries to capture data from the virtual machine.

Query: 
   ```
    InsightsMetrics
    | where TimeGenerated > ago(1h)
    | where Name == "UtilizationPercentage"
    | summarize avg(Val) by bin(TimeGenerated, 5m), Computer //split up by computer
    | render timechart
   ```
![Screenshot of Azure Monitor log queries](../media/az104-lab11-architecture.png)


## Key takeaways

+ Alerts help you detect and address issues before users notice there might be a problem with your infrastructure or application.
+ You can alert on any metric or log data source in the Azure Monitor data platform.
+ An alert rule monitors your data and captures a signal that indicates something is happening on the specified resource.
+ An alert is triggered if the conditions of the alert rule are met. Several actions (email, SMS, push, voice) can be triggered.
+ Action groups include individuals that should be notified of an alert.
