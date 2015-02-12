# The Lombiq Hosting Suite - the technology behind DotNest that you can use in your own application



## Overview

The Lombiq Hosting Suite is a set of Orchard modules, an Azure Cloud Service implementation, a continuous integration environment, many different practices and know-how that deal with various aspects of hosting Orchard applications in efficient, maintainable ways. It drives DotNest too but you can use it to improve your own application: to benefit from the features of the Hosting Suite you can incorporate it into your application without moving your sites to DotNest. On a high level the Hosting Suite enables the following:

- The webserver running Orchard can be fully stateless: apart from the application itself and occasional cache files nothing is stored on the web server (as opposed to Orchard storing data in numerous files). This makes Orchard a lot easier to deploy, maintain and scale.
- Enhanced multi-server support: Orchard is fully capable of running on multiple webservers, nodes won't go out of sync. Furthermore it's simple to spin new nodes up with a standardized Orchard deployment package, enabling to throttle throughput in a flexible manner.
- Enhanced Azure support: Azure-specific implementations make Orchard run better on Azure. This includes e.g. implementations that shift local file system storage to Blob storage.
- Due to the above deployments and other maintenance tasks can be run [without downtime](http://dotnest.com/blog/99-99-uptime-in-dotnest-s-first-month).
- Enhanced multi-tenancy: as the engine behind DotNest, the Hosting Suite adds services to efficiently host hundreds of sites from the same Orchard application while giving flexible ways of managing them, even through a web API. Site shell settings are stored in the database instead of config files and sites are started on demand, on their first hit, instead of being started on app start. These features improve maintainability and dramatically reduce startup time while increasing achievable site density.
- Improved performance: among other performance improvements the Hosting Suite also features a reverse proxy component that offers offloaded Orchard-optimized output caching.
- Designed to be extensible, the Suite exposes many events and other extension points for developers to use. Features, although enhancing each other, can be used and turned on or off independently.


**How can the Hosting Suite help me? [Ask us](http://lombiq.com/contact-us) at Lombiq and we'll work it out for you!**


## Detailed features, per components of the Hosting Suite

### Shell management

The low-level core of the Hosting Suite is Lombiq.Hosting.ShellManagement, containing the following features:

- An extended shell settings manager that stores shell settings in the database. With this Orchard gets rid of Settings.txt files stored in App_Data, thus removing the need to maintain and back up these files.
- Exposes fine-grained events that can be used to subscribe to shell settings changes, even if those are invoked from another shell. Such events can be used to extend shell management in arbitrary ways.
- Adds the ability to have a common database for all sites where the connection string is stored only once, not per site as usual. This makes it possible to move the application to a different database (like between a staging and production environment) by changing only one connection string.
- On demand shell activation: shells are only activated if a request arrives to their site. This dramatically reduces app start time and conserves computing resources.


### Multi-tenancy

The below modules of the Hosting Suite greatly improve Orchard's multi-site capabilities.

- Lombiq.Hosting.MultiTenancy: exposes hosting services, both as service classes and web services, for managing a multi site Orchard environment.
    - Site management services and a RESTful (authenticated) web API for fetching, creating, updating, removing and setting up sites.
    - Adds the ability to set an isolated database environment for sites by putting sites' tables under a different SQL Server schema, accessed through an automatically generated user.
    - Exposes services for managing sites that run Lombiq.Hosting.MultiTenancy.Tenants (see below).
    - Has services for running batches of maintenance steps for sites that can also be started from a web API endpoint. This can be used to change sites in bulk (like enabling modules, changing settings, running upgrades).
    - Exposes various events and configuration points for extending functionality.
    - Extends Lombiq.Hosting.DistributedEvents with a file watching event raising service for instant event propagation that can be used with shared file systems.
    - Makes the site management admin page only fetch what it displays and adds paging for long lists of sites. This removes any limitation on the number of sites that can be managed from the admin UI. Site management UI also got some further improvements, e.g. the ability to jump to a site by using its name, the ability to edit all shell settings (not just the default ones) and the ability to remove sites from the UI.
    - Adds UI for logging in as the superuser of a site for administrative purposes.
- Lombiq.Hosting.MultiTenancy.Tenants: runs on the sites of a hosting environment. Provides the following services:
    - Feature guard: prevents configured features from being turned on or off on sites, providing configurable constraints on what sites can do.
    - Storage quota management: continuously updates media storage usage data and enforces a configured storage quota.
    - Runtime quota management: enforces a configured runtime quota, i.e. it will shut down shells after a period of inactivity, conserving computing resources (think app pool idle timeout for sites) and dramatically improving site density.
    - Exposes a way to log in as the superuser of the site for administrative purposes.
    - Exposes various events and configuration points for extending functionality.
- Lombiq.Hosting.MultiTenancy.Bridge: provides interoperability between Lombiq.Hosting and Lombiq.Hosting.Tenants. Hosting and Hosting.Tenants are not coupled but play together through the common interfaces defined in Bridge.

### Stateless webservers

The Lombiq.Hosting.Stateless module adds general features for making the Orchard webserver stateless, enhancing horizontal scalability and improving maintainability. Azure-related modules listed below also provide features for removing the need for persistent storage on the webserver.

- Makes the Reports module not store anything in App\_Data.
- Makes indexing services don't store anything in App\_Data.

### Azure services

The following modules enhance Orchard when run on Azure.

- Lombiq.Hosting.Azure: contains extensions for the Hosting Suite for enhancing it on Azure.
    - The media folder of the site is removed from Blob Storage when a site is removed.
    - Lucene search indices are removed from blob Storage when a site is removed.
- [Lombiq.Hosting.Azure.Indexing](https://orchardazureindexing.codeplex.com/): extends Orchard's search indexing services to optimize them for Azure.
    - Enables Lucene indexing to use Blob storage (with local cache) with [AzureDirectory](https://github.com/richorama/AzureDirectory).

### Multi-server support

By bridging an important gap that prevents Orchard being run on a multi-server (multi-node/Web Farm) setup the [Lombiq.Hosting.DistributedEvents](https://orcharddistributedevents.codeplex.com/) module adds the ability to broadcast events between server nodes.

- Generic extensible services for event broadcasting.
- Implementations for broadcasting shell events and signals. Thus server nodes are always in sync, even if data is stored in the otherwise instance-local CacheManager. This means changes in e.g. content type definitions, feature states or roles will propagate to other server nodes.

### Application maintenance

The below modules improve how maintenance tasks can be executed on Orchard applications.

- [Lombiq.Hosting.Readonly](http://orchardreadonly.codeplex.com/): adds the ability to set a site into read-only mode (i.e. no content can be saved but the site is viewable normally). This enables safe deployment scenarios where before updating the application its database is backed up, as Readonly prevents data loss in such a transitional state.
- [Lombiq.Hosting.RecipeRemoteExecutor](http://reciperemoteexecutor.codeplex.com/): allows to execute recipes (for a single site or for multiple sites in a multi-site setup) through an authenticated web API. This is a lightweight option to make automatable changes to Orchard sites remotely.

### No down-time deployment

All our websites, including DotNest is deployed and maintained using a technology package that provides the ability to push updates into production seamlessly with a few clicks. This technology package is set of PowerShell scripts, which means that it can be easily integrated with any continuous integration software. You can also use it independently of the Hosting Suite (and the Hosting Suite can be utilized without this deployment package), but they create a powerful toolkit in terms of application maintenance when used together. The deployment package can be utilized with Azure Web Sites (its real power can be harnessed with the staged publishing feature) and Azure SQL. The most important features are:

- Easy usage and customizability: you can easily manage any number of Azure Web Sites across multiple Azure subscriptions. Each script is highly parameterized so that you can adapt their behaviour according to the current situation.
- Database backup from Azure SQL.
- Ability to replace the staging database with the production one, so you can test your application in the staging environment with up-to-date data.
- Swapping the staging environment out to production, which includes updating the App Settings and Connection Strings in both environments.

### Reverse cache proxy

Part of the Hosting Suite is a reverse proxy that sits in front of the web servers as an Azure Cloud Service. It is taking off work off the web servers by doing any preliminary routing and doing full output caching and gzip compression. Doing output caching on the reverse proxy frees up memory on the webservers and greatly improves performance.

Despite of output caching any content change on the Orchard sites is immediately visible to users, because cache entries are automatically evicted on the proxy server when content is published. This logic is handled by the Lombiq.Hosting.RequestRouter.Adapter module.

### Extensions

Some other features extending the above ones in the Lombiq.Hosting.Extensions module:

- Distributed event raising implementation using a file system watcher for Distributed Events that can be used in environments where the file system is shared among server nodes (usable e.g. on Azure Web Sites or with a shared storage folder).
- HTTP module for restoring the original HTTP Host header if the application is behind a reverse proxy.


## Getting the Hosting Suite

Distributed Events, Readonly, Recipe Remote Executor and Azure Indexing are open source modules and free to use. Lombiq offers commercial-grade support for integrating and extending these modules. The other parts of the Hosting Suite are proprietary and Lombiq offers flexible licenses for them.

**How can the Hosting Suite help me? [Ask us](http://lombiq.com/contact-us) at Lombiq and we'll work it out for you!**