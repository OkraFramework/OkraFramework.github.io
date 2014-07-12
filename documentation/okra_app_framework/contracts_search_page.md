---
layout: documentation
title: Adding a search page
---

Adding a search page
====================

NB: Also see [http://andyonwpf.blogspot.co.uk/2012/10/app-search-and-okra-app-framework.html](http://andyonwpf.blogspot.co.uk/2012/10/app-search-and-okra-app-framework.html)

The Okra App Framework makes it easy to add a search page to your Windows Store application. Advantages of using the framework include,

* Straightforward implemetation when following the MVVM pattern
* Automatically follows the Windows search design guidelines with respect to activation and navigation
* Improved performance by attaching to the SearchPane.QuerySubmitted event where possible

Thankfully these intricacies are handled by the framework and all you need to do is to provide the desired UI and business logic.

Creating a search page with the Visual Studio templates
-------------------------------------------------------

As part of the Visual Studio extension for the Okra App Framework, an MVVM based search page template is provided (see [Creating pages with
the Visual Studio templates](navigation_creating_pages_with_templates.html) for more information on the page templates).

1. From the Visual Studio solution explorer select Add New Item.

2. Select "Search Contract (MVVM)" and click Add.

3. Initialize the search manager by adding the following import to your application bootstrapper,

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
    [Import]
    public ISearchManager SearchManager { get; set; }

    ...
}
{% endhighlight %}

4. Replace all references to 'SampleDataItem' in the search page view-model with a model class to represent individual search results.

5. Insert custom code where prompted (look for the "TODO" comments) in the PerformQuery(...) and UpdateResults(...) methods to return the list of sub-categories and the items for a subcategory respectively.

Creating a search page manually
-------------------------------

Alternatively creating a search page manually follows the same pattern as for any other page. You can use this approach when you wish to
use a UI that is significantly different to the default search template. The steps to follow are,

1. Initialize the search manager by adding the following import to your application bootstrapper,

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
    [Import]
    public ISearchManager SearchManager { get; set; }

    ...
}
{% endhighlight %}

2. (to be continued!)

TODO (include manifest changes)
