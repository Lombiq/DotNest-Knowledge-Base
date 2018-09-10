# The Lombiq Hosting Suite - the technology behind DotNest that you can use in your own application



## Overview

The Lombiq Hosting Suite is a set of Orchard modules, an Azure Cloud Service implementation, a continuous integration environment, many different practices and know-how that deal with various aspects of hosting Orchard applications in efficient, maintainable ways. It drives DotNest too but you can use it to improve your own application: to benefit from the features of the Hosting Suite you can incorporate it into your application without moving your sites to DotNest. On a high level the Hosting Suite enables the following:

- The webserver running Orchard can be fully stateless: apart from the application itself and occasional cache files nothing is stored on the web server (as opposed to Orchard storing data in numerous files). This makes Orchard a lot easier to deploy, maintain and scale.
- Enhanced multi-server support: Orchard is fully capable of running on multiple webservers, nodes won't go out of sync. Furthermore it's simple to spin new nodes up with a standardized Orchard deployment package, enabling to throttle throughput in a flexible manner.
- Enhanced Azure support: Azure-specific implementations make Orchard run better on Azure. This includes e.g. implementations that shift local file system storage to Blob storage.
- Due to the above deployments and other maintenance tasks can be run [without downtime](http://dotnest.com/blog/99-99-uptime-in-dotnest-s-first-month), you can do [blue-green deployments](https://martinfowler.com/bliki/BlueGreenDeployment.html).
- Enhanced multi-tenancy: as the engine behind DotNest, the Hosting Suite adds services to efficiently host hundreds of tenants from the same Orchard application while giving flexible ways of managing them, even through a web API. Tenant shell settings are stored in the database instead of config files and tenants are started on demand, on their first hit, instead of being started on app start. These features improve maintainability and dramatically reduce startup time while increasing achievable site density.
- Improved performance: among other performance improvements the Hosting Suite also features a reverse proxy component that offers offloaded Orchard-optimized output caching.
- A continuous integration/deployment environment fully integrated with Orchard to support agile DevOps workflows. Among others it can be used to build and deploy the app, swap the staging environment out to production without users noticing it and to copy data over from the production environment to staging. And your developers get anonymized/depersonalized DB snapshots (required for GDPR compliance) delivered to a repository!
- Designed to be extensible, the Suite exposes many events and other extension points for developers to use. Features, although enhancing each other, can be used and turned on or off independently.

And you can retrofit any existing Orchard app with the Hosting Suite!

**How can the Hosting Suite help me? [Ask us](http://lombiq.com/contact-us) at Lombiq and we'll work it out for you!**


## Detailed features, per components of the Hosting Suite

### Shell management

The low-level core of the Hosting Suite is Lombiq.Hosting.ShellManagement, containing the following features:

- An extended shell settings manager that stores shell settings in the database. With this Orchard gets rid of Settings.txt files stored in App_Data, thus removing the need to maintain and back up these files.
- Exposes fine-grained events that can be used to subscribe to shell settings changes, even if those are invoked from another shell. Such events can be used to extend shell management in arbitrary ways.
- Adds the ability to have a common database for all tenants where the connection string is stored only once, not per tenant as usual. This makes it possible to move the application to a different database (like between a staging and production environment) by changing only one connection string.
- On demand shell activation: shells are only activated if a request arrives to their tenant. This dramatically reduces app start time and conserves computing resources.


### Multi-tenancy

The below modules of the Hosting Suite greatly improve Orchard's multi-tenant capabilities.

- Lombiq.Hosting.MultiTenancy: exposes hosting services, both as service classes and web services, for managing a multi tenant Orchard environment.
    - Tenant management services and a RESTful (authenticated) web API for fetching, creating, updating, removing and setting up tenants.
    - Adds the ability to set an isolated database environment for tenants by putting tenants' tables under a different SQL Server schema, accessed through an automatically generated user.
    - Exposes services for managing tenants that run Lombiq.Hosting.MultiTenancy.Tenants (see below).
    - Has services for running batches of maintenance steps for tenants that can also be started from a web API endpoint. This can be used to change tenants in bulk (like enabling modules, changing settings, running upgrades).
    - Exposes various events and configuration points for extending functionality.
    - Extends Lombiq.Hosting.DistributedEvents with a file watching event raising service for instant event propagation that can be used with shared file systems.
    - Makes the tenant management admin page only fetch what it displays and adds paging for long lists of tenants. This removes any limitation on the number of tenants that can be managed from the admin UI. Tenant management UI also got some further improvements, e.g. the ability to jump to a tenant by using its name, the ability to edit all shell settings (not just the default ones) and the ability to remove tenants from the UI.
    - Adds UI for logging in as the superuser of a tenant for administrative purposes.
    - Displays which tenants are running (started by on demand on their first hit) and which ones are sleeping.
    - Displays the last occurrence date of the important activities (user logins, content modifications) on the tenants. Also, displays the number of these events occurred per month.
    - Displays on how many tenants a module is enabled to learn which are the popular ones.
    - Gives the ability to list the tenants by their Media folder sizes or by the number of content items.
- Lombiq.Hosting.MultiTenancy.Tenants: runs on the tenants of a hosting environment. Provides the following services:
    - Feature guard: prevents configured features from being turned on or off on tenants, providing configurable constraints on what tenants can do.
    - Storage quota management: continuously updates media storage usage data and enforces a configured storage quota.
    - Runtime quota management: enforces a configured runtime quota, i.e. it will shut down shells after a period of inactivity, conserving computing resources (think app pool idle timeout for tenants) and dramatically improving tenant density.
    - Exposes a way to log in as the superuser of the tenant for administrative purposes.
    - Exposes various events and configuration points for extending functionality.
- Lombiq.Hosting.MultiTenancy.Bridge: provides interoperability between Lombiq.Hosting and Lombiq.Hosting.Tenants. Hosting and Hosting.Tenants are not coupled but play together through the common interfaces defined in Bridge.

### Stateless webservers

The Lombiq.Hosting.Stateless module adds general features for making the Orchard webserver stateless, enhancing horizontal scalability and improving maintainability. Azure-related modules listed below also provide features for removing the need for persistent storage on the webserver.

- Makes the Reports module not store anything in App\_Data.
- Makes indexing services don't store anything in App\_Data.

### Azure services

The following modules enhance Orchard when run on Azure.

- Lombiq.Hosting.Azure: contains extensions for the Hosting Suite for enhancing it on Azure.
    - The media folder of the tenant is removed from Blob Storage when a tenant is removed.
    - Lucene search indices are removed from blob Storage when a tenant is removed.
- [Lombiq.Hosting.Azure.Indexing](https://github.com/Lombiq/Orchard-Azure-Indexing): extends Orchard's search indexing services to optimize them for Azure.
    - Enables Lucene indexing to use Blob storage (with local cache) with [AzureDirectory](https://github.com/richorama/AzureDirectory).
- [Lombiq.Hosting.Azure.ApplicationInsights](https://github.com/Lombiq/Orchard-Azure-Application-Insights): easy integration of [Azure Application Insights](http://azure.microsoft.com/en-us/documentation/articles/app-insights-start-monitoring-app-health-usage/), the Azure application telemetry service.

### Multi-server support

By bridging an important gap that prevents Orchard being run on a multi-server (multi-node/Web Farm) setup the [Lombiq.Hosting.DistributedEvents](https://github.com/Lombiq/Orchard-Distributed-Events) module adds the ability to broadcast events between server nodes.

- Generic extensible services for event broadcasting.
- Implementations for broadcasting shell events and signals. Thus server nodes are always in sync, even if data is stored in the otherwise instance-local CacheManager. This means changes in e.g. content type definitions, feature states or roles will propagate to other server nodes.

### Application maintenance

The below modules improve how maintenance tasks can be executed on Orchard applications.

- [Lombiq.Hosting.Readonly](https://github.com/Lombiq/Orchard-Read-only): adds the ability to set a site into read-only mode (i.e. no content can be saved but the site is viewable normally). This enables safe deployment scenarios where before updating the application its database is backed up, as Readonly prevents data loss in such a transitional state.
- [Lombiq.Hosting.RecipeRemoteExecutor](https://github.com/Lombiq/Orchard-Recipe-Remote-Executor): allows to execute recipes (for a single tenant or for multiple tenants in a multi-tenant setup) through an authenticated web API. This is a lightweight option to make automatable changes to Orchard sites remotely.

### No down-time deployment and continuous integration

All our websites, including DotNest is deployed and maintained using a [TeamCity](https://www.jetbrains.com/teamcity/)-integrated technology package that provides the ability to push updates into production seamlessly with a few clicks. This technology package is set of PowerShell scripts, which means that it can be easily integrated with any continuous integration software. You can also use it independently of the Hosting Suite (and the Hosting Suite can be utilized without this deployment package), but they create a powerful toolkit in terms of application maintenance when used together. The deployment package can be utilized with Azure App Services (its real power can be harnessed with the staged publishing feature) and Azure SQL databases. The most important features are:

- Swapping the staging environment out to production, which includes updating the App Settings and Connection Strings in both environments. With this you can push out new versions of the app without users noticing anything.
- Automated periodic anonymized/depersonalized (or otherwise transformed) DB and Media snapshots pushed to a repository so developers can always test with the latest data from production. Complying with GDPR and other data protection regulations you can prevent developers seeing actual user data.
- Ability to replace the staging database and Media folder with the production one, so you can test your application in the staging environment with up-to-date data.
- Easy on-demand collection of all Orchard and Azure logs that developers can download in a ZIP file for troubleshooting.
- Easy usage and customizability: you can easily manage any number of Azure App Services across multiple Azure subscriptions. Each script is highly parameterized so that you can adapt their behaviour according to the current situation.

### Reverse cache proxy

Part of the Hosting Suite is a reverse proxy that sits in front of the web servers as an Azure Cloud Service. It is taking off work off the web servers by doing any preliminary routing and doing full output caching and gzip compression. Doing output caching on the reverse proxy frees up memory on the webservers and greatly improves performance.

Despite of output caching any content change on the Orchard sites is immediately visible to users, because cache entries are automatically evicted on the proxy server when content is published. This logic is handled by the Lombiq.Hosting.RequestRouter.Adapter module.

### Extensions

Some other features extending the above ones in the Lombiq.Hosting.Extensions module:

- Distributed event raising implementation using a file system watcher for Distributed Events that can be used in environments where the file system is shared among server nodes (usable e.g. on Azure App Services or with a shared storage folder).
- HTTP module for restoring the original HTTP Host header if the application is behind a reverse proxy.

### Safe custom per-tenant theming with Media Theme

With the Media Theme feature of the Hosting Suite it's possible to provide full theming capabilities to each of the tenants in a multi-tenant Orchard application without the use of standard theme projects: Media Themes, similar to normal themes, can be developed in Visual Studio (or another IDE) then deployed from source control to tenants. Media Theme is also [available on DotNest](theming/).

Such themes can use a programming model safe even in a completely public SaaS like DotNest: each tenant can have its own theme with [Liquid markup templates](https://shopify.github.io/liquid/), CSS, JS and graphic files and anything else client-side, without endangering other tenants. Site owners can thus add their own themes and you don't need to have a separate theme project in your app's solution for each of them!

### Algolia hosted search

The standard Lucene Orchard search with can be swapped out with the hosted [Algolia search engine](http://algolia.com/) which provides very fast instant-like search. The [Algolia Search module](https://github.com/Lombiq/Orchard-Algolia-Search) provides an easy way to plug Algolia into Orchard. If you want to check out how such search works, see the [Orchard Algolia Search Demo site](https://algoliasearchdemo.dotnest.com) here on DotNest.


## Getting the Hosting Suite

The linked modules are open source modules and free to use. Lombiq offers commercial-grade support for integrating and extending these modules. The other parts of the Hosting Suite are proprietary and Lombiq offers flexible licenses for them.

**How can the Hosting Suite help me? [Ask us](http://lombiq.com/contact-us) at Lombiq and we'll work it out for you!**
