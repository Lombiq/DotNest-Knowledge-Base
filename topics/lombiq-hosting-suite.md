# The Lombiq Hosting Suite - the technology behind DotNest that you can use in your own application



The Lombiq Hosting Suite is a set of modules that deal with various aspect of hosting Orchard applications in efficient, maintainable ways. It drives DotNest too but you can use it to improve your own application: to benefit from the features of the Hosting Suite you can incorporate it into your application without moving your sites to DotNest. On a high level the Hosting Suite enables the following:

- The webserver running Orchard can be fully stateless: apart from the application itself and occasional cache files nothing is stored on the web server (as opposed to Orchard storing data in numerous files). This makes Orchard a lot easier to deploy, maintain and scale.
- Enhanced multi-server support: Orchard is fully capable of running on multiple webservers, nodes won't go out of sync. Furthermore it's simple to spin new nodes up with a standardized Orchard deployment package, enabling to throttle throughput in a flexible manner.
- Enhanced Azure support: Azure-specific implementations make Orchard run better on Azure. This includes e.g. implementations that shift local file system storage to Blob storage.
- Enhanced multi-tenancy: as the engine behind DotNest, the Hosting Suite adds services to efficiently host hundreds of tenants from the same Orchard application while giving flexible ways of managing them, even through a web API. Tenant shell settings are stored in the database instead of config files and tenants are started on demand, on their first hit, instead of being started on app start. These features improve maintainability and dramatically reduce startup time while increasing achievable site density.
- Designed to be extensible, the Suite exposes many events and other extension points for developers to use. Features, although enhancing each other, can be used and turned on or off independently.

Detailed features, per modules of the Hosting Suite:

- Lombiq.Hosting.ShellManagement: the low-level core of the Hosting Suite, containing the following features:
    - An extended shell settings manager that stores shell settings in the database. With this Orchard gets rid of Settings.txt files stored in App_Data, thus removing the need to maintain and back up these files.
    - Exposes fine-grained events that can be used to subscribe to shell settings changes, even if those are invoked from another shell. Such events can be used to extend shell management in arbitrary ways.
    - Adds the ability to have a common database for all tenants where the connection string is stored only once, not per tenant as usual. This makes it possible to move the application to a different database (like between a staging and production environment) by changing only one connection string.
    - On demand shell activation: shells are only activated if a request arrives to their tenant. This dramatically reduces app start time and conserves computing resources.
- Lombiq.Hosting: contains some generic services related to hosting Orchard.
	- An HTTP module for restoring the original host header for Orchard if the site runs behind a reverse proxy that changes the host header.
	- An event raising implementation for Lombiq.Hosting.DistributedEvents that uses a file system watcher on shared files to notify server nodes of an event (usable e.g. on Azure Web Sites or with a shared storage folder).
- Lombiq.Hosting.MultiTenancy: exposes hosting services, both as service classes and web services, for managing a multi tenant Orchard environment.
    - Tenant management services and a RESTful (authenticated) web API for fetching, creating, updating, removing and setting up tenants.
    - Adds the ability to set an isolated database environment for tenants by putting tenants' tables under a different SQL Server schema, accessed through an automatically generated user.
    - Exposes services for managing tenants that run Lombiq.Hosting.Tenants.
    - Has services for running batches of maintenance steps for tenants that can also be started from a web API endpoint. This can be used to change tenants in bulk (like enabling modules, changing settings, running upgrades).
    - Exposes various events and configuration points for extending functionality.
    - Extends Lombiq.Hosting.DistributedEvents with a file watching event raising service for instant event propagation that can be used with shared file systems.
    - Makes the tenant management admin page only fetch what it displays and adds paging for long lists of tenants. This removes any limitation on the number of tenants that can be managed from the admin UI.
    - Adds UI for logging in as the superuser of a tenant for administrative purposes.
- Lombiq.Hosting.MultiTenancy.Tenants: runs on the tenants of a hosting environment. Provides the following services:
    - Feature guard: prevents configured features from being turned on or off on tenants, providing configurable constraints on what tenants can do.
    - Storage quota management: continuously updates media storage usage data and enforces a configured storage quota.
    - Runtime quota management: enforces a configured runtime quota, i.e. it will shut down shells after a period of inactivity, conserving computing resources (think app pool idle timeout for tenants) and dramatically improving tenant density.
    - Exposes a way to log in as the superuser of the tenant for administrative purposes.
    - Exposes various events and configuration points for extending functionality.
- Lombiq.Hosting.MultiTenancy.Bridge: provides interoperability between Lombiq.Hosting and Lombiq.Hosting.Tenants. Hosting and Hosting.Tenants are not coupled but play together through the common interfaces defined in Bridge.
- Lombiq.Hosting.Stateless: adds general features for making the Orchard webserver stateless.
    - Makes the Reports module not store anything in App\_Data.
    - Makes indexing services don't store anything in App\_Data.
- Lombiq.Hosting.Azure: contains extensions for the Hosting Suite for enhancing it on Azure.
    - The media folder of the tenant is removed from Blob Storage when a tenant is removed.
    - Lucene search indices are removed from blob Storage when a tenant is removed.
- Lombiq.Hosting.Azure.Indexing: extends Orchard's search indexing services to optimize them for Azure.
    - Enables Lucene indexing to use Blob storage (with local cache) with [AzureDirectory](https://github.com/richorama/AzureDirectory).
- [Lombiq.Hosting.DistributedEvents](https://orcharddistributedevents.codeplex.com/): adds the ability to broadcast events between nodes of a multi-server environment.
    - Generic extensible services for event broadcasting.
    - Implementations for broadcasting shell events and signals. Thus server nodes are thus always in sync, even if data is stored in the otherwise instance-local CacheManager. This means changes in e.g. content type definitions, feature states or roles will propagate to other server nodes.
- [Lombiq.Hosting.Readonly](http://orchardreadonly.codeplex.com/): adds the ability to set a site into read-only mode (i.e. no content can be saved but the site is viewable normally). This enables safe deployment scenarios where before updating the application its database is backed up, as Readonly prevents data loss in such a transitional state.
- [Lombiq.Hosting.RecipeRemoteExecutor](http://reciperemoteexecutor.codeplex.com/): allows to execute recipes (for a single tenant or for multiple tenants in a multi-tenant setup) through an authenticated web API. This is a lightweight option to make automatable changes to Orchard sites remotely.

DistributedEvents, Readonly and RecipeRemoteExecutor are open source modules and free to use. Lombiq offers commercial-grade support for integrating and extending these modules. The other modules are proprietary and Lombiq offers flexible licenses for them.

How can the Hosting Suite help me? [Ask us](http://lombiq.com/contact-us) at Lombiq and we'll work it out for you!