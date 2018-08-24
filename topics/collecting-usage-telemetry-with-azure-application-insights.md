# Collecting usage telemetry of your DotNest sites with Azure Application Insights



Do you want to know how your DotNest sites perform? How fast they are, who your users are, whether there are any client-side errors? Among others this is what [Azure Application Insights](http://azure.microsoft.com/en-us/documentation/articles/app-insights-start-monitoring-app-health-usage/) can answer: it's an application telemetry service that can collect various data from your application and present you in a useful form.

To use AI:

1.  Create a Microsoft Azure account and create an AI account in it as described on the [Azure website](http://azure.microsoft.com/en-us/documentation/articles/app-insights-get-started/). Both are free!
2.  Retrieve your AI instrumentation key from the Azure Portal and configure it on your site's admin (under Settings/Azure Application Insights).
3.  That's it! Telemetry data will now be collected for your site.

This will give you insights in your site's usage, including:

- How many people are visiting your site, where from, with which browser, etc.
- How fast is your site.
- Whether there are any client-side (JavaScript) exceptions. Since you can [completely theme your site](theming/) you can also write complex JavaScript so knowing about such errors can help a lot.