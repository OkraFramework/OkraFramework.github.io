---
layout: documentation
title: The Okra App Framework Bootstrapper
---

The Okra App Framework Bootstrapper
===================================

All applications built using the Okra App Framework include a bootstrapper class whose responsibility it is to initialise any services
required (both framework services and custom services) and to coordinate activation of the application with the framework.

The good news is that the framework provides a base implementation **OkraBootstrapper** that provides most of this functionality.
Therefore for many applications the bootstrapper will simply be defined as,

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
}
{% endhighlight %}

Adding additional services
--------------------------

Since by default the Okra App Framework uses MEF for composition of the application, you can add additional services to the bootstrapper as MEF imports. These
will be often be services provided by the framework, however custom services may be included in the same manner. In either case the framework will ensure that
all services are created prior to the first application activation being processed.

For example, to add search functionality to an application you would import the **ISearchManager** service in the bootstrapper as follows,

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
    [Import]
    public ISearchManager SearchManager { get; set; }
}
{% endhighlight %}

Initializing services
---------------------
The bootstrapper is also the best place to perform any service initialization required prior to activation of the application. The default
**OkraBootstrapper** base class provides a **SetupServices()** method that can be overriden for this purpose.

For example, to specify a non-default search page name you would do this as follows,

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
    [Import]
    public ISearchManager SearchManager { get; set; }

    protected override void SetupServices()
    {
        SearchManager.SearchPageName = "MySearchPage";
    }
}
{% endhighlight %}
