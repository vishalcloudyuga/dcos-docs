---
post_title: Deployments
menu_order: 5
---

This guide provides an introduction to DC/OS service deployments. It will show you how to deploy new versions of a DC/OS service without downtime.

Blue-green deployments allow you to safely deploy applications that are serving live traffic. Blue-green deployment eliminates application downtime and allows you to quickly roll back to the blue version of the application if necessary.

The basic methodology is that you have two versions of an application: blue and green. One version of your application is live (e.g. blue) and the other is in staging (e.g. green) as you prepare a new release. 

For example, if blue is live and green is staging, when you are ready to deploy a new version of the application you follow this sequence:
 
 1.  Drain all traffic, requests, and pending operations from the blue version of the application.
 1.  Switch to the green version.
 1.  Turn off the blue version. 

Blue-green deployments give you the flexibility to rollback deployments if anything goes wrong.

**Prerequisites:**

*  [DC/OS](/docs/1.8/administration/installing/) installed with at least one [private agents](/docs/1.8/overview/concepts/).
*  [DC/OS CLI](/docs/1.8/usage/cli/install/) installed.
*  The CLI JSON processor [jq](https://github.com/stedolan/jq/wiki/Installation) installed.






