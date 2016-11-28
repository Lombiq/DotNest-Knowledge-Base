# Writing a theme from scratch



Again you have two options:

- Implementing a theme by saving the CSS, JS and Placement from the admin UI with the Theme Override module
- Developing an Orchard theme as usual in you favorite IDE (preferably Visual Studio) and optionally deploying it from source control automatically. **If you're a hardcore developer, you may want to use this approach!**


## Building a theme from the admin UI

You can use the [Theme Override module](https://github.com/Lombiq/Orchard-Theme-Override) to edit the site's CSS, JS and Placement from the admin UI.

First make the DotNest Empty theme the current theme, since this theme includes no styling at all. Then enable the Theme Override module and edit the styling from under Theme Override Settings in Themes.

You can upload graphics or other static files through Media Library and reference them with their URL from CSS/JS. For this you can follow some conventions for better maintainability like placing your theme-related files into the Theme (or Themes/MyTheme) folder in media.

Note that **CSS font-faces referencing font files won't work** because of how static files are loaded from a cookie-less domain on DotNest. Instead of referencing files in your `font-face` declarations please 1) [embed the fonts as data URIs](http://sosweetcreative.com/2613/font-face-and-base64-data-uri) and 2) place these into a separate, separately loaded CSS file (e.g. fonts.css) which you then have to exclude from Combinator processing (see Settings/Combinator on the admin). So if your fonts are embedded into CSS and are loaded from a separate file that Combinator won't touch then you're good to go! For an example of this technique see the [Orchard Ambassadors theme](https://bitbucket.org/Lombiq/orchard-ambassadors-theme).

You can even override shape templates by enabling the Liquid Markup Templates feature and writing templates in the [Liquid Markup language](http://liquidmarkup.org/) through the built-in Templates module (see [the documentation of the Orchard Liquid Markup module](https://github.com/Lombiq/Orchard-Liquid-Markup) on what you can do with it).


## Developing a theme and deploying it to your site with Media Theme

DotNest sites include the Media Theme theme and the Media Theme Deployment module: the theme enables you to extract a theme to the MediaTheme folder in Media (with regular Placement.info, Scripts and Styles folder) and use those. Just set the MediaTheme as the current theme; if you want to have automatic deployment for such theme packages then also enable the Media Theme Deployment module and configure it from Media Theme Deployment Configuration under Themes.

You can just use the Media Theme theme and store your theme files (stylesheets, scripts, Placement.info) under the MediaTheme folder in Media Library but it's better to use the theme together with the Media Theme Deployment module so you can take advantage of the automatic theme package deployment, outlined below.

You can set up a URL to a zip file to be deployed as the theme. You can use this to deploy from your source control or from storage providers like Dropbox. For Bitbucket the theme package URL can be the URL of the automatically generated zip file for an arbitrary branch of the repository. Also you can use the hook URL (displayed on the admin UI of MediaTheme) to invoke a deploy when you push to your repository.

The zip package that should be deployed should contain the full folder hierarchy of the theme. The hierarchy can start in the root of the package or also deeper. However packages MUST include a Theme.txt in the folder their theme folder structure starts from and there should be only one Theme.txt in the whole package. (E.g. this is a valid theme structure in MyTheme.zip: 201403/Theme.txt, 201403/Styles/site.css...)

The theme package should follow these conventions:

- Place the favicon into Images/favicon.ico and it will be automatically picked up (optional). This works only if you don't override the Layout shape; if you do then you need to add the favicon from Liquid.
- If you use any, place stylesheets into the Styles and scripts into the Scripts folder (just as usual).
- Placement.info should be on the same level as the Theme.txt file (optional).

For using stylesheets and scripts you can simply include the common stylesheet in Styles/site.css (you can still develop with multiple stylesheets for a better structure, just in the end bundle them to a single file e.g. by using the import statement of [LESS](http://lesscss.org/)), scripts in Scripts/site-head.js and Scripts/site-foot.js (for head and foot scripts, respectively; you can bundle multiple scripts with e.g. [TypeScript](http://www.typescriptlang.org/)'s reference statement). This works only if you don't override the Layout shape; you can, however, include an arbitrary set of stylesheets and scripts in an arbitrary order from templates as usual with Liquid, see below.

You can even override shape templates by adding templates to your theme written in the [Liquid Markup language](http://liquidmarkup.org/) (just add .liquid files in the same way you'd add .cshtml files). You can even include static resources from such templates in arbitrary ways. See [the documentation of the Orchard Liquid Markup module](https://github.com/Lombiq/Orchard-Liquid-Markup) for more information on what you can do with such templates.

If you want to test your theme locally during development you can also do that .[Install Orchard locally](http://docs.orchardproject.net/en/latest/Documentation/Installing-Orchard/) as usual (preferably use the same version that runs on DotNest) and develop the theme:

- Add your theme as a standard theme project and do the development as outlined above.
- Also add the [Liquid Markup module](https://github.com/Lombiq/Orchard-Liquid-Markup) if you want to create Liquid templates. Enable its "Liquid Markup View Engine" feature to get Liquid support, then you'll be able to add Liquid templates as files to your theme just as you would with Razor templates.
- Add any modules available on DotNest that you intend to use for theme development.
- You can synchronize content between your DotNest site and the local installation with the Import Export module using recipes.

For samples on how such a theme looks see:

- [Student Partner Blog Theme](https://bitbucket.org/barthamark/student-partner-blog-theme/) for [http://mspblog.hu/](http://mspblog.hu/).
- [NativeHungarian.com theme's repository](https://bitbucket.org/lehoczky_zoltan/native-hungarian-theme). The theme is automatically deployed from the repository to the [Native Hungarian website](http://nativehungarian.com/), running on DotNest.
- Another example theme by Abhishek Luv can be accessed under [its GitHub repository](https://github.com/abhishekluv/mydotnesttheme) (the theme can also be deployed from GitHub).
- [Elemental Gankery Theme](https://bitbucket.org/benedekfarkas/elemental-gankery-media-theme/overview) for [http://elementalgankery.dotnest.com/](http://elementalgankery.dotnest.com/).
- [Spring Harvest Challenge Theme](https://bitbucket.org/Lombiq/orchard-spring-harvest-challenge-theme) for [http://harvestchallenge.net/](http://harvestchallenge.net/).
- [Orchard Ambassadors theme](https://bitbucket.org/Lombiq/orchard-ambassadors-theme) for [http://ambassadors.orchardproject.net/](http://ambassadors.orchardproject.net/).

When developing the theme locally you can import the [test content export files](http://orcharddojo.net/orchard-resources/Library/Utilities/TestContent/) from the Dojo Library to instantly get a site full of sample content.