---
layout: documentation
title: Navigation between pages
---

Navigation between pages
========================

There are two general cases to consider for navigation,

* For some standard pages (e.g. application home page, search page, ...) the Okra App Framework will automatically handle navigation
at the appropriate times. See the documentation for each of these features for more information.
* Otherwise you can programatically navigate from one page of you application to another as detailed below.

Navigating between pages with INavigationBase
---------------------------------------------

The first step when implementing page navigation is to obtain a navigation manager. Whilst Okra supports multiple navigation
contexts, you can access the current navigation manager by importing an **INavigationContext** and calling its **GetCurrent()** method.

For example you would add the following constructor to your view-model,

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

You can now use the methods on the navigation manager to navigate between pages of your application.

To navigate to a new page you call
the **NavigateTo(...)** method with the name of the destination page (there are also extension methods to support using
types for page names). The Okra App Framework will coordinate the discovery, creation and navigation to the specified page.

{% highlight c# %}
public void NavigateToPage2()
{
    this.navigationManager.NavigateTo("Page 2");
}
{% endhighlight %}

To return back through the navigation stack you simply call the **GoBack()** method.

{% highlight c# %}
public void NavigateBack()
{
    this.navigationManager.GoBack();
}
{% endhighlight %}

Programatically determining if you can navigate
-----------------------------------------------

When calling the **NavigateTo(...)** method it is expected that the specified page exists. If the page has not been defined then
an exception is throw. Alternatively you can call the **CanNavigateTo(...)** method to return a boolean value representing whether the
specified page exists.

Similarly there is a **CanGoBack** property and associated **CanGoBackChanged** event for determining whether the navigation manager will allow back navigation.