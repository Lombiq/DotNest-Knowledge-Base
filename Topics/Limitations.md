# Limitations for DotNest sites



Due to the multi-tenant architecture of DotNest while you can do a lot with your Orchard sites there are certain limitations needed to keep the environment stable and all of the users protected.

- You can't install custom modules. This is a limitation of Orchard's multi-tenancy. It is to prevent tenant users from running malicious code that could endanger the whole service.
- You can't install custom themes. However you can still completely style your site: [learn more about theming on DotNest](Theming).
- You can't use certain built-in modules because they are either not meaningful in a multi-tenant setup or could endanger the service. Deprecated modules (like Orchard.Media) are also not supported.
- You won't see Warmup, Output Cache or Upgrade either. This is because all of these are managed for you.