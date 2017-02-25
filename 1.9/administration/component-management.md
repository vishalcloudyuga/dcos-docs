---
post_title: Component Management
menu_order: 5.5
---

The DC/OS Component Package Manager (Pkgpanda) installs and manages DC/OS components on individual DC/OS nodes.

The DC/OS Component Package Manager (Pkgpanda) is not designed to be used by operators of DC/OS but rather by the DC/OS components themselves when performing node lifecycle events, like installing, upgrading, and uninstalling.

DC/OS components are bundled into component packages that are compressed at build time, included as part of the DC/OS installer, transported from the bootstrap machine to the desired node by the installer, and decompressed to a specific location on each node during setup. Generally these component packages contain one or more systemd unit definitions, binaries, and configuration files.

# Component health

Component health is monitored by the DC/OS Diagnostics (3DT) component. For more information about component monitoring, see [Monitoring](/docs/1.9/administration/monitoring/).

# Component logs

Component logs are sent to journald and exposed by the DC/OS Log component. For more infromation about component logs, see [Logging](/docs/1.9/administration/logging/).

# API reference

The Component Management API is exposed through Admin Router under the `/pkgpanda/` path.

<div class="swagger-section">
  <div id="message-bar" class="swagger-ui-wrap message-success" data-sw-translate=""></div>
  <div id="swagger-ui-container" class="swagger-ui-wrap" data-api="/docs/1.9/api/pkgpanda.yaml">

  <div class="info" id="api_info">
    <div class="info_title">Loading docs...</div>
  <div class="info_description markdown"></div>
</div>
