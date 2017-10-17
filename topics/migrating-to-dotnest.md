# Migrating your existing Orchard site to DotNest



We're glad you're thinking about moving your existing site to DotNest! Running your site on DotNest can save you from a lot of maintenance headaches. The process shouldn't be too complicated:

1. Make sure DotNest offers everything your site needs. If you miss some module or theme badly you can request it too!
2. Export content on the old site and import it into your DotNest site with the Import/Export module. Make sure to remove any references to modules that are not available on DotNest.
3. If you have Media files then a little more work is needed:
	- The files are served from a different URL than on your original site so URLs need to be changed. Most possibly on your old site Media files were under something like */Media/Default/image.jpg* but DotNest will serve them from under *https://dotneststatic.com/media/yoursite/image.jpg* ("yoursite" here is your site's name, the same as in your site's domain name before ".dotnest.com"). So you need to a) change all URLs that were embedded in texts (like Body texts) in the export file (just do a search and replace) and b) change the `FolderPath`s of exported Media content items to only include forward slashes ("/") instead of backslashes ("\").
	- Files can't be export/imported (yet), so share them with us via [email](https://dotnest.com/contact-us).
4. Migrate your theme: you can use a variety of approaches, see the [theming documentation](theming/).

[Contact us](/contact-us) if you're unsure whether your site can be moved to DotNest or not! Also get in touch with us if you have a lot of content or media and would request our assistance in the migration.