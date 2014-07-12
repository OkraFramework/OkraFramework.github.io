---
layout: documentation
title: Navigation between pages
---

Navigation between pages
========================

Once you have defined the pages for your application you need some way to display them. This can further be split into two scenarios,

* For a set of standard pages (e.g. application home page, search page, ...) the Okra App Framework will automatically handlenavigation
at the appropriate times. All you need to do is name the page with one of the SpecialPageNames constants, and in some cases add the
respective Okra service to your bootstrapper.
* In other cases where you may want to programatically navigate from one page of you application to another - you do this using the
INavigationContext and INavigationBase interfaces.

Navigating between pages with INavigationBase
---------------------------------------------

The first step when you want to implement navigation from a page is to import a navigation manager implementing the **INavigationBase**
interface. Since Okra supports multiple navigation contexts (e.g. main application, settings panes as well as more advanced navigation scenarios),
you access the appropriate navigation manager via the **INavigationContext.GetCurrent()** method. For example you would add the following
constructor to your view-model,

{% highlight c# %}
[ViewModelExport("MyPageName")]
public class MyViewModel : MyViewModelBase
{
    private readonly INavigationBase navigationManager;

    [ImportingConstructor]
    public MyViewModel(INavigationContext navigationContext)
    {
        this.navigationManager = navigationContext.GetCurrent();
    }
}
{% endhighlight %}

You can now use the methods on the **INavigationBase** instance to navigate between pages of your application. The Okra App Framework will manage
the discovery, creation and navigation to the page specified in the **NavigateTo(...)** method (there are also extension methods to support using
types for page names). For example,

{% highlight c# %}
public void NavigateToPage2()
{
    this.navigationManager.NavigateTo("Page 2");
}
{% endhighlight %}

And to return back through the navigation stack you simply call the **GoBack()** method. The Okra App Framework will handle storage, lifetime management
and persistence of the navigation stack for you. For example,

{% highlight c# %}
public void NavigateBack()
{
    this.navigationManager.GoBack();
}
{% endhighlight %}

Programatically determining if you can navigate
-----------------------------------------------

When calling the **NavigateTo(...)** method it is anticipated that the specified page is defined with the appropriate attributes applied. If the page
does not exist an exception is throw. Alternatively you can call the **CanNavigateTo(...)** method to return a boolean value representing whether the
specified page exists.

Similarly there is a **CanGoBack** property and associated **CanGoBackChanged** event for determining whether the navigation manager will allow back navigation.