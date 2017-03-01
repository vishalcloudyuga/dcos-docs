---
post_title: Debugging from the DC/OS GUI
feature_maturity: experimental
menu_order: 20
---

You can also debug your service or pod from the DC/OS GUI.

## Service and Pod Health Summaries

The DC/OS dashboard displays a **Services Health** box that tells you at a glance whether your service or pod is healthy. The **Services** -> **Services** page lists each service and its status. Possible statuses are `Deploying`, `Waiting`, or `Running`.

## Debugging Page

Clicking the name of a service or pod and then the `Debug` tab reveals a detailed debugging page. There, you will see sections for **Last Changes**, **Last Task Failure**, **Task Statistics**, **Recent Resource Offers**, a **Summary** of resource offers and what percentage of those offers matched your pod or service's requirements, and a **Details** section that lists the host where your servive or pod is running and which resource offers were successful and unsuccessful for each deployment. You can use the information on this page to learn where and how you need to modify your service or pod definition.

![Debug Screen](/docs/1.9/usage/debugging/img/debug-ui.png)
