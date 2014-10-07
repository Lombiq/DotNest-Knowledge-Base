# Theming a DotNest site



Although you can't install custom themes on DotNest (see "[Limitations](limitations)") you still have a lot of control over theming. You can even develop usual Orchard themes in Visual Studio and get them automatically deployed from source control!

There are basically two options to style your site on DotNest:

- Extending a theme available on DotNest
- Writing a theme from scratch


## Extending a theme available on DotNest

If you are satisfied with what a theme that is available on DotNest ([see which themes are available](available-modules-and-themes)) you can use that as a base and extend it. Just enable the theme you want to build on ([Pretty Good Bootstrap Base Theme](https://pgbootstrapbasetheme.codeplex.com/) is a good base theme for example) and also enable the [Theme Override module](https://themeoverride.codeplex.com/). You can then modify the styling under Theme Override Settings in Themes freely (including CSS, JS and Placement). You can watch our [tutorial video](https://www.youtube.com/watch?v=edrIvSMa9Aw&list=PLuskKJW0FhJcXpbKqATKllLj9RsH-eDg3&index=3) about using Theme Override too!

You can even override shape templates by enabling the Liquid Markup Templates feature and writing templates in the [Liquid Markup language](http://liquidmarkup.org/) through the built-in Templates module (see [the documentation of the Orchard Liquid Markup module](https://orchardliquid.codeplex.com/documentation) on what you can do with it).


## Writing a theme from scratch

Again you have two options:

- Implementing a theme by saving the CSS, JS and Placement from the admin UI with the Theme Override module
- Developing an Orchard theme as usual in you favorite IDE (preferably Visual Studio) and optionally deploying it from source control automatically. **If you're a hardcore developer, you may want to use this approach!**

### Building a theme from the admin UI

You can use the [Theme Override module](https://themeoverride.codeplex.com/) to edit the site's CSS, JS and Placement from the admin UI.

First make the DotNest Empty theme the current theme, since this theme includes no styling at all. Then enable the Theme Override module and edit the styling from under Theme Override Settings in Themes.

You can upload graphics or other static files through Media Library and reference them with their URL from CSS/JS. For this you can follow some conventions for better maintainability like placing your theme-related files into the Theme (or Themes/MyTheme) folder in media.

You can even override shape templates by enabling the Liquid Markup Templates feature and writing templates in the [Liquid Markup language](http://liquidmarkup.org/) through the built-in Templates module (see [the documentation of the Orchard Liquid Markup module](https://orchardliquid.codeplex.com/documentation) on what you can do with it).

### Developing a theme and deploying it to your site with Media Theme

DotNest sites include the Media Theme theme and the Media Theme Deployment module: the theme enables you to extract a theme to the MediaTheme folder in Media (with regular Placement.info, Scripts and Styles folder) and use those. Just set the MediaTheme as the current theme; if you want to have automatic deployment for such theme packages then also enable the Media Theme Deployment module and configure it from Media Theme Deployment Configuration under Themes.

You can just use the Media Theme theme and store your theme files (stylesheets, scripts, Placement.info) under the MediaTheme folder in Media Library but it's better to use the theme together with the Media Theme Deployment module so you can take advantage of the automatic theme package deployment, outlined below.

You can set up an URL to a zip file to be deployed as the theme. You can use this to deploy from your source control or from storage providers like Dropbox. For Bitbucket the theme package URL can be the URL of the automatically generated zip file for an arbitrary branch of the repository. Also you can use the hook URL (displayed on the admin UI of MediaTheme) to invoke a deploy when you push to your repository.

The zip package that should be deployed should contain the full folder hierarchy of the theme. The hierarchy can start in the root of the package or also deeper. However packages MUST include a Theme.txt in the folder their theme folder structure starts from and there should be only one Theme.txt in the whole package. (E.g. this is a valid theme structure in MyTheme.zip: 201403/Theme.txt, 201403/Styles/site.css...)

The theme package should follow these conventions:

- Place the favicon into Images/favicon.ico and it will be automatically picked up (optional).
- If you use any, place stylesheets into the Styles and scripts into the Scripts folder (just as usual).
- Placement.info should be on the same level as the Theme.txt file (optional).

For using stylesheets and scripts you can simply include the common stylesheet in Styles/site.css (you can still develop with multiple stylesheets for a better structure, just in the end bundle them to a single file e.g. by using the import statement of [LESS](http://lesscss.org/)), scripts in Scripts/site-head.js and Scripts/site-foot.js (for head and foot scripts, respectively; you can bundle multiple scripts with e.g. [TypeScript](http://www.typescriptlang.org/)'s reference statement).

You can, however, include an arbitrary set of common stylesheets and scripts in an arbitrary order by adding a **single** .cshtml file to your theme: you can use this template (e.g. a Layout.cshtml file or a Resources.cshtml file for [Pretty Good Bootstrap Base Theme](https://pgbootstrapbasetheme.codeplex.com/)) to include any stylesheet or script with Style/Script.Include/Require() (you can also chain AtHead() or AtFoot() to them for scripts). Be aware that this template will only be parsed but not executed: any other statement (or any other method chained onto the resource inclusion calls) will be omitted.

You can even override shape templates by adding templates to your theme written in the [Liquid Markup language](http://liquidmarkup.org/) (just add .liquid files in the same way you'd add .cshtml files). You can even include static resources from such templates in arbitrary ways. See [the documentation of the Orchard Liquid Markup module](https://orchardliquid.codeplex.com/documentation) for more information on what you can do with such templates.

For a sample on how such a theme looks see the [NativeHungarian.com theme's repository](https://bitbucket.org/lehoczky_zoltan/native-hungarian-theme). The theme is automatically deployed from the repository to the [Native Hungarian website](http://nativehungarian.com/), running on DotNest.

When developing the theme locally you can import the [test content export file](http://orcharddojo.net/orchard-resources/Library/Utilities/TestContent/)s from the Dojo Library to instantly get a site full of sample content.