# Under the hood of DotNest



So how does DotNest work?

DotNest is a multi-tenant [Orchard](http://orchardproject.net) service. This means that there is a single Orchard application that we maintain and upgrade while all of the websites of DotNest run on it.

Orchard's multi-tenancy is powerful but in its basic form it's not enough to run a service like DotNest. We enhanced the built-in multi-tenancy capability with a lot of important features to power DotNest, for example:

- The webserver is completely stateless, not storing anything besides the application code and some cache files. To achieve this we moved everything (even Lucene search index files) that was stored on the local file system either to the database or to cloud storage. This makes it possible to scale out DotNest easily.
- Tenant shell settings are also not stored on the file system anymore but in the database.
- Tenants are started on demand, when the first request hits them, not on application start. This dramatically reduces app startup time.
- Tenants can also be shut down due to inactivity to conserve computing resources. This makes DotNest more responsive and stable.
- Every runtime data that is not already in a distributed cache is propagated through all the server nodes if there are multiple servers. Thus Orchard is completely multi-node (web farm) compatible.

All this is part of a suite of Orchard modules called the Lombiq Hosting Suite. [You can learn more about the Lombiq Hosting Suite here](/lombiq-hosting-suite).

DotNest sites don't need Warmup as they are always kept warm. Also there is no need to do output caching from Orchard because a reverse proxy is sitting before the web servers, caching unauthenticated responses.

DotNest runs on Azure, Microsoft's flexible, reliable, high-performance cloud platform.