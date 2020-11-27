---
title: API Gateway and API Manager 7.7 November 2020 Release Notes
linkTitle: API Gateway and API Manager November 2020
weight: 60
date: 2020-11-05
---
## Summary

API Gateway is available as a software installation or a virtualized deployment in Docker containers. API Manager is a licensed product running on top of API Gateway, and has the same deployment options as API Gateway.

The software installation is available on Linux. For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

Docker deployment is supported on Linux. For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

## New features and enhancements

The following new features and enhancements are available in this update.

### Casandra backup and restore automated script

An easy and convenient automated script to backup and restore Cassandra clusters are now included in API Manager. The script will also be available, separately, for customers in supported versions prior to November 2020. For more information, see [Apache Cassandra backup and restore](/docs/cass_admin/cassandra_bur).

### Enhanced API life cycle management for Organization administrators

In the [September 2020](/docs/apim_relnotes/20200930_apimgr_relnotes/#organization-administrators-can-publish-apis) release, we created the `api.manager.orgadmin.selfservice.enabled` system property to allow an Organization administrator to publish and unpublish APIs that were created in their organization without approval from an API Administrator.

In this release, we enhanced the property with new API life cycle events (deprecate, undeprecate, upgrade, and grant access to APIs), which will be made available to an Organization administrator when `api.manager.orgadmin.selfservice.enabled` is set to True.

For more information, see [System property changes](/docs/apim_reference/system_props/).

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.

<!-- Use this section to describe any changes in the behavior of the product (as a result of features or fixes), for example, new Java system properties in the jvm.xml file. This section could also be used for any important information that doesn't fit elsewhere. -->

### placeholder heading for Important changes

placeholder text for Important changes

## Deprecated features

<!-- As part of our software development life cycle we constantly review our API Management offering. As part of this review, no capabilities have been deprecated. -->

As part of our software development life cycle we constantly review our API Management offering.

The following capabilities have been deprecated.

### placeholder heading for Deprecated features

placeholder text for Deprecated features

## Removed features

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, no capabilities have been removed -->

To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed:

### placeholder heading for Removed features

placeholder text for Removed features

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

<!-- Insert table of issues here -->

### Other fixed issues

<!-- Insert table of issues here -->

## Known issues

The following are known issues for this update.

<!-- Insert table of issues here -->

### Policy Studio help documentation

This update includes changes to the Policy Studio documentation, but the version of the documentation (Eclipse Help) shipped with Policy Studio is out of sync with the online documentation and does not include these changes.

We are working to rectify this issue in a future update. In the meantime, you can find the up to date documentation online at [Develop in Policy Studio](/docs/apim_policydev/).

## Update a classic (non-container) deployment

To update your API Gateway, see [Update from API Gateway One Version](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/).

To upgrade from an older version of the product, see [Upgrade from API Gateway 7.5.x or 7.6.x](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/).

## Update a container deployment

Any custom `fed` files deployed to a container must be upgraded using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics#upgradeconfig-options) or [projupgrade](/docs/apim_reference/devopstools_ref#projupgrade-command-options). They must be upgraded the same way, regardless of whether they are API Manager enabled or not.

The `fed` now contains the updates for the API Manager configuration and can be used to build containers.

## Documentation

This section describes documentation enhancements and related documentation.

### Documentation enhancements

The following new instructions and enhancements are available in the documentation.

* placeholder.
* placeholder.

### API Management open source documentation

The latest version of API Gateway, API Manager, and API Portal documentation has been migrated to Markdown format and is available in a [public GitHub repository](https://github.com/Axway/axway-open-docs) to prepare for future collaboration using an open source model. As part of this migration, the documentation has been restructured to help users navigate the content and find the information they are looking for more easily.

Documentation change history is now stored in GitHub. To see details of changes on any page, click the link in the **Last modified** section at the bottom of the page.

### Related documentation

To find all available documentation for this product version:

1. Go to [Manuals on the Axway Documentation portal](https://docs.axway.com/bundle).
2. In the left pane Filters list, select your product or product version.

Customers with active support contracts need to log in to access restricted content.

The following reference documents are also available:

* [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en) - Lists the different operating systems, databases, browsers, and thick client platforms supported by each Axway product.
* [Interoperability Matrix](https://docs.axway.com/bundle/Axway_Products_InteroperabilityMatrix_allOS_en) - Provides product version and interoperability information for Axway products.

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email [support@axway.com](mailto:support@axway.com) or visit [Axway Support](https://support.axway.com/).

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.
