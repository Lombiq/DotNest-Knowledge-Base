# GDPR compliance



DotNest is compliant with the [EU GDPR](https://www.eugdpr.org/) (General Data Protection Regulation). If you're a company and an owner of a DotNest site you need to make your visitors aware of DotNest's [privacy policy](https://lombiq.com/privacy-policy) and ask for their consent to data collection.

**Looking to make an on-premise Orchard site GDPR compliant?** The DevOps framework behind DotNest, the [Lombiq Hosting Suite](lombiq-hosting-suite) can help you with that, so can our team. Just [get in touch with us](/contact-us)!

Please note that all DotNest sites gather anonymous usage statistics so we have data to improve the platform. Such data is collected in accordance of our [privacy policy](https://lombiq.com/privacy-policy) using Google Analytics and Azure Application Insights. As an owner of a DotNest site it's your responsibility to make your visitors aware about this and any further data collection, cookies’ usage taking place on this site and guarantee the site’s compliance with GDPR.

To help you with this we've included the [Lombiq.Privacy module](https://github.com/Lombiq/Orchard-Privacy). Note that using the module won't make your site automatically GDPR-compliant, you'll need to create a privacy policy for your site at least as well and link it from the various consent-collecting texts.


## Privacy consent banner for any data collection and analytics

Enable the Lombiq.Privacy module's "Lombiq Privacy - Consent banner" feature which will display a privacy consent banner on your site. This is needed if any data collection takes place on the site.

The text of the banner is configurable from under the Orchard site settings; you'll need to create a privacy policy for your site at least and link it from the banner.

If you're using a custom analytics script on your site then you may only enable its data collection if the user has given consent via the consent banner. You can check for the consent using the `ConsentCookie` service. Below is an example for Google Analytics.

    <!-- Global site tag (gtag.js) - Google Analytics -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=UA-xxxxxxxx-xx"></script>
    <script>
        if (!window['user-is-authenticated'] && typeof ConsentCookie !== 'undefined' && !ConsentCookie.HasConsentCookie()) {
            window['ga-disable-UA-xxxxxxxx-xx'] = true;
        }
        window.dataLayer = window.dataLayer || [];
        function gtag(){dataLayer.push(arguments);}
        gtag('js', new Date());

        gtag('config', 'UA-xxxxxxxx-xx');
    </script>   
 
The check in the first line will disable tracking if the user hasn't given consent. This check won't run (`window["user-is-authenticated"]` will be `false` or `undefined`) if the user is authenticated: The reason is that if you let users register then you should also enable the "Lombiq Privacy - Registration consent" feature (see below), thus every authenticated user will have already given consent.


## Registration consent

To collect consent on user registration enable the "Lombiq Privacy - Registration consent" feature. This will display a consent checkbox with some text on the registration screen. Just as with the consent banner feature you have to configure the displayed text.


## Consent checkbox on any form

If you're using custom forms on your website created with the Dynamic Forms modules (or display content item editors on the frontend) then make sure to also enable the "Lombiq Privacy - Form consent" feature and add the privacy consent-asking Element of it to your forms.

If you have commenting enabled on your site then also add the `ConsentCheckboxPart` to the Comment content type.

Note that both kind of checkboxes will only be displayed if the user has not already given consent to the data collection.