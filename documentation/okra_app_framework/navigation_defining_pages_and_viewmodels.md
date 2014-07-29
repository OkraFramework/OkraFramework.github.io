---
layout: documentation
title: Defining pages and view-models
---

Defining pages and view-models
==============================

Windows Store applications based upon the Okra App Framework generally consist of a number of interconnected pages starting from a central hub,
or home page. The user navigates forward through the application flow by clicking images, buttons or links, and can also navigate backwards by
using a back button that is placed in the top left hand corner of each page.

By default the Okra App Framework defines pages with a unique name using a declarative attribute-based approach ([an alternative convention based
approach is also possible](navigation_convention_based_discovery.html)). The framework will then use MEF (the built in .Net composition framework) to locate the view and
view-model associated with this page name, create and initialise instances of these and wire them together as appropriate.

For the Okra App Framework to find a page it should be annotated with the **PageExportAttribute**,

{% highlight c# %}
[PageExport("MyPageName")]
public sealed partial class MyPage : Page
{
    public MyPage()
    {
        this.InitializeComponent();
    }
}
{% endhighlight %}

Simliarly the corresponding view-model is annotated with the **ViewModelExportAttribute**,

{% highlight c# %}
[ViewModelExport("MyPageName")]
public class MyViewModel : MyViewModelBase
{
    ...
}
{% endhighlight %}

Special page names
------------------

The Okra App Framework also provides a set of predefined page names as part of the **Okra.Navigation.SpecialPageNames** class. These are used as
default page names for a number of the services provided as part of the framework. The SpecialPageNames values can be used anywhere that a string-based page name
is used. For example to define the home page for an application,

{% highlight c# %}
[PageExport(SpecialPageNames.Home)]
public sealed partial class MyHomePage : Page
{
    ...
}
{% endhighlight %}

The provided values are detailed below,

<table class="table ">
	<thead>
		<tr><th>Value</th><th>Usage</th><th>Default value for</th></tr>
	</thead>
	<tbody>
		<tr><td>Home</td><td>Specifies the default page at application startup</td><td>INavigationManager.HomePageName</td></tr>
		<tr><td>Search</td><td>Specifies the application search page</td><td>ISearchManager.SearchPageName</td></tr>
		<tr><td>ShareTarget</td><td>Specifies the page to display for a share target</td><td>IShareTargetManager.ShareTargetPageName</td></tr>
	</tbody>
</table>

Using types as page names
-------------------------

As an alternative to string-based page names you can also use types for a strongly-typed approach. Note that the same type must be used for
both the view and view-model.

{% highlight c# %}
[PageExport(typeof(MyViewModel))]
public sealed partial class MyPage : Page
{
    ...
}

[ViewModelExport(typeof(MyViewModel))]
public class MyViewModel : MyViewModelBase
{
    ...
}
{% endhighlight %}

In some parts of the framework overloads for type-based page names are not available. In these cases there is a **PageName.FromType(...)** helper method that can be
used to convert to the corresponding string-based page name. For example,

{% highlight c# %}
NavigationManager.HomePageName = PageName.FromType(typeof(MyViewModel));
{% endhighlight %}