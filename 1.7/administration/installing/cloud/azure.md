---
post_title: Install DC/OS on Azure
nav_title: Azure
menu_order: 1
---

This document explains how to install DC/OS 1.7 through the Azure Marketplace.

TIP: To get support on Azure Marketplace-related questions, join the Azure Marketplace [Slack community](http://join.marketplace.azure.com).

# System requirements

## Hardware

To [use](/docs/1.7/usage/) all of the services offered in DC/OS, you should choose at least five Mesos Agents using `Standard_D2` [Virtual Machines](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/), which is the default size in the DC/OS Azure Marketplace offering.

Selecting smaller-sized VMs is not recommended, and selecting fewer VMs will likely cause certain resource-intensive services such as distributed datastores not to work properly (from installation issues to operational limitations).

## Software

You will need an active [Azure subscription](https://azure.microsoft.com/en-us/pricing/purchase-options/) to install DC/OS via the Azure Marketplace.

Also, to access nodes in the DC/OS cluster you will need `ssh` installed and configured.

# Install DC/OS

To install DC/OS 1.7 on Azure, use the [Azure Resource Manager templates](https://downloads.dcos.io/dcos/EarlyAccess/commit/14509fe1e7899f439527fb39867194c7a425c771/azure.html) provided.

## Next steps

- [Add users to your cluster][10]
- [Install the DC/OS Command-Line Interface (CLI)][1]
- [Use your cluster][4]
- [Scaling considerations][3]

[1]: /docs/1.7/usage/cli/install/
[3]: https://azure.microsoft.com/en-us/documentation/articles/best-practices-auto-scaling/
[4]: /docs/1.7/usage/
[10]: /docs/1.7/administration/user-management/
