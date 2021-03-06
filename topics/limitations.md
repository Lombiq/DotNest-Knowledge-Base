# Limitations for DotNest sites



Due to the multi-tenant architecture of DotNest while you can do a lot with your Orchard sites there are certain limitations needed to keep the environment stable and all of the users protected.

- You can't install custom modules. This is a limitation of Orchard's multi-tenancy: It is to prevent site users from running malicious code that could endanger the whole service. However there is a [wide variety of pre-installed modules](available-modules-and-themes) available.
- You can't install custom themes, however you can still completely style your site: [learn more about theming on DotNest](theming/). Also there is a [variety of pre-installed themes](available-modules-and-themes) available.
- You can't use certain built-in modules because they are either not meaningful in a multi-tenant setup or could endanger the stability of the service. Deprecated modules (like Orchard.Media) are not supported either. If certain features of a module are allowed but not all features than you'll see also the non-allowed features listed on the admin site unfortunately. When you try to enabled such features they will just not get enabled.
- You won't see Warmup, Output Cache or Upgrade either. This is because all of these are managed for you.

If you are looking for the convenience of DotNest but want to build your own Orchard app with custom modules and themes then our Orchard operations as a service is for you: [contact us](https://dotnest.com/contact-us) and we can operate your Orchard with the technology that drives DotNest!