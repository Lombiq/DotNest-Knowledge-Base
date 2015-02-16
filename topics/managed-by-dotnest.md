# What is automatically managed by DotNest for your site?



Since DotNest is a multi-tenant environment there are certain aspects we manage for you in an optimized way. This means you don't have to care about a lot of maintenance!

- Output caching: there is no need to do output caching from Orchard because a reverse proxy is sitting before the web servers, caching unauthenticated responses. Cache entries are kept for 20 minutes but for Orchard content pages entries are immediately evicted when content is published.
- Warmup: DotNest sites don't need Warmup as they are always kept warm.
- Orchard upgrades/updates: DotNest sites always run on the latest stable Orchard version.
- Data upgrades: you don't need to run data upgrades (i.e. the ones from the Upgrade module) as DotNest automatically runs those when needed.
- SMTP settings: unless you insist on using your own SMTP server for some reason the SMTP settings on DotNest sites are pre-populated to ensure your site can send e-mails out of the box.
- Static content delivery: static content (e.g. stylesheets, scripts, media files) are all served from a cookie-less domain to give better performance.