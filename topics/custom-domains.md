# Using a custom domain name with DotNest



On DotNest you can use your custom domain name with your site too instead of the default mysite.dotnest.com form. To use this feature you have to do the following:

1. Buy a domain name at a domain registrator.
2. Configure the domain to use it with DotNest. For this you need to use the admin panel your domain registrator provides your and configure domains the following way (make sure to wait up to a few hours for such changes to propagate through the domain name system):
	- For second-level domains (i.e. if you want to use something like mydomain.com for your website) set the IP of the domain A record to 191.238.49.139. This is the standard way to do it, so you have to include this IP in your domain record (what is unfortunate because we may change this IP if it's inevitable). To overcome this you could use services that offer CNAME-like functionality at the root record, so you can point mydomain.com to your DotNest subdomain (e.g. mysite.dotnest.com) instead (such services are available from [DNSimple](http://support.dnsimple.com/articles/alias-record), [DNS Made Easy](http://www.dnsmadeeasy.com/services/aname-records/) and [easyDNS](http://docs.easydns.com/aname-records/) for example).
	- For lower-level domains (like blog.mydomain.com) set the CNAME record to the default DotNest subdomain of your site (e.g. mysite.dotnest.com). If you use a second-level domain make sure to set the www subdomain (e.g. www.mydomain.com) as a CNAME to your second-level domain.
3. Configure your custom domain on DotNest for your site from under the tenant management page.

Your site now should listen to your custom domain, enjoy!