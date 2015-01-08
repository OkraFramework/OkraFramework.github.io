---
layout: documentation
title: Adding a search page
---

Adding a search page
====================

<div class="alert alert-danger">
	<b>Warning:</b> As of Windows 8.1 it is no longer recommended to use the search contract for
	in app search (<a href="http://msdn.microsoft.com/en-us/library/windows/apps/xaml/JJ130767(v=win.10).aspx">see
	the SearchBox control instead</a>). This feature is included in the Okra App Framework for legacy use only.
</div>

The Okra App Framework makes it easy to add a search page to your Windows Store application. Advantages of using the framework include,

* Straightforward implemetation when following the MVVM pattern
* Automatically follows the Windows search design guidelines with respect to activation and navigation
* Improved performance by attaching to the SearchPane.QuerySubmitted event where possible

Thankfully these intricacies are handled by the framework and all you need to do is to provide the desired UI and business logic.

Creating a search page
----------------------

Initialize the search manager by adding the following import to your application bootstrapper,

{% highlight c# %}
using Okra.Search;

public class AppBootstrapper : OkraBootstrapper
{
    [Import]
    public ISearchManager SearchManager { get; set; }

    ...
}
{% endhighlight %}

Open the 'Package.appxmanifest' file for the solution. In the 'Declarations' tab add the 'Search' declaration and
   save changes to the 'Package.appxmanifest'

Add a new page and view-model to the project that will be used as a search page

Add **PageExport** and **ViewModelExport** attributes naming the page **SpecialPageNames.Search**

{% highlight c# %}
[PageExport(SpecialPageNames.Search)]
public sealed partial class MySearchPage : Page
{
    ...
}

[ViewModelExport(SpecialPageNames.Search)]
public class MySearchViewModel
{
    ...
}
{% endhighlight %}

The view-model should also implement the **Okra.Search.ISearchPage** interface. This interface defines a single **PerformQuery(...)** method
that is called every time a search is performed.
Note that this may be called multiple times for a single page instance, since if the user performs subsequent searches then
the page should be updated each time.

{% highlight c# %}
[ViewModelExport(SpecialPageNames.Search)]
public class MySearchViewModel : Okra.Search.ISearchPage
{
    ...

    public void PerformQuery(string queryText, string language)
    {
        throw new NotImplementedException();
    }
}
{% endhighlight %}

