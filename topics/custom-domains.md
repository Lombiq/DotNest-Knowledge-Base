# Using a custom domain name with DotNest



On DotNest you can use your custom domain name with your site too instead of the default mysite.dotnest.com form. To use this feature you have to do the following:

1. Buy a domain name at a domain registrator.
2. Configure the domain to use it with DotNest. For this you need to use the admin panel your domain registrator provides you and configure domains the following way (make sure to wait up to a few hours for such changes to propagate through the domain name system):
	- For second-level domains (i.e. if you want to use something like mydomain.com for your website) set the IP of the domain A record to 191.238.49.139. This is the standard way to do it, so you have to include this IP in your domain record (what is unfortunate because we may change this IP if it's inevitable). To overcome this you could use services that offer CNAME-like functionality at the root record, so you can point mydomain.com to your DotNest subdomain (e.g. mysite.dotnest.com) instead (such services are available from [DNSimple](http://support.dnsimple.com/articles/alias-record), [DNS Made Easy](http://www.dnsmadeeasy.com/services/aname-records/) and [easyDNS](http://docs.easydns.com/aname-records/) for example).
	- If you use a second-level domain make sure to set the www subdomain (e.g. www.mydomain.com) as a CNAME to your second-level domain. Note that DotNest tenants can only have www-less domains: a request to the www version will be redirected to the www-less one.
	- For lower-level domains (like blog.mydomain.com) set the CNAME record to the default DotNest subdomain of your site (e.g. mysite.dotnest.com).
3. Configure your custom domain on DotNest for your site from under [the tenant management page](/DotNest.Frontend/UserTenantManagement/). Note that custom domain usage is subject to a fee, see [pricing details](/pricing).

Your site now should listen to your custom domain, enjoy!

Once you set up your custom domain any requests to the standard DotNest subdomain will be redirected to your custom domain (i.e. if users open your site with the old mysite.dotnest.com address then they will get redirected to mydomain.com). Exception is a secure request: if the request goes through SSL to your site then that won't get redirected. The reason is that SSL is not available for custom domains so this way you can still administer your site securely.