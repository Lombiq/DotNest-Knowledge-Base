# GDPR compliance



DotNest is compliant with the [EU GDPR](https://www.eugdpr.org/) (General Data Protection Regulation). If you're a company and an owner of a DotNest site you need to make your visitors aware of DotNest's [privacy policy](https://lombiq.com/privacy-policy) and ask for their consent to data collection.

Please note that all DotNest sites gather anonymous usage statistics so we have data to improve the platform. Such data is collected in accordance of our [privacy policy](https://lombiq.com/privacy-policy) using Google Analytics and Azure Application Insights. As an owner of a DotNest site it's your responsibility to make your visitors aware about this and any further data collection, cookies’ usage taking place on this site and guarantee the site’s compliance with GDPR.

To help you with this we've included the [Lombiq.Privacy module](https://github.com/Lombiq/Orchard-Privacy): After enabling its "Lombiq Privacy - Consent banner" feature it'll display a privacy consent banner on your site. The text of the banner is configurable from under the Orchard site settings.

If you're using a custom Analytics script on your site then you may only enable its data collection if the user has given consent via the consent banner. You can check for the consent using the `ConsentCookie` service. Below is an example for Google Analytics.

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
 
The check in the first line will disable tracking if the user hasn't given consent. This check won't run (`window["user-is-authenticated"]` will be `false` or `undefined`) if the user is authenticated: The reason is that if you let users register then you should also enable the "Lombiq Privacy - Registration consent" feature, thus every authenticated user will have already given consent.

If you're using custom forms on your website created with the Dynamic Forms modules then make sure to also enable the "Lombiq Privacy - Form consent" feature and add the privacy consent-asking Element of it to your forms.