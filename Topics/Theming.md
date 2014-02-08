# Theming a DotNest site



Although you can't install custom themes on DotNest (see "[Limitations](Limitations)") you still have a lot of control over theming.

There are basically two options to style your site on DotNest:

- Extending a theme available on DotNest
- Writing a theme from scratch


## Extending a theme available on DotNest

If you are satisfied with what a theme that is available on DotNest ([see which themes are available](AvailableModulesAndThemes)) you can use that as a base and extend it. Just enable the theme you want to build on ([Pretty Good Bootstrap Base Theme](https://pgbootstrapbasetheme.codeplex.com/) is a good base theme for example) and also enable the Theme Override module. You can then add some CSS to modify the styling under Theme Override Settings in Themes.


## Writing a theme from scratch

Make the DotNest Empty theme the current theme first. This theme includes no styling and all.

Then you can use the Theme Overide module (just enable it) to supply all of the styling yourself. After enabling the module you can save the CSS you want (and JS and also Placement) under Theme Override Settings in Themes.

You can upload graphics or other static files you the Media Library and reference them with their URL from CSS/JS. For this you can follow some conventions for better maintainability like placing your theme-related files into the Theme (or Themes/MyTheme) folder in media.