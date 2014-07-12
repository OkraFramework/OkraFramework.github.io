---
layout: documentation
title: Navigation using commands
---

Navigation using commands
=========================

In most cases navigation between pages within an application will occur based upon some user input, for example clicking a button.
In MVVM applications this is usually performed by the button data-binding to a command on the view-model (i.e. an implementation
of the **ICommand** interface). Whilst you can easily write your own navigation commands using the DelegateCommand class, the Okra App
Framework also provides dedicated commands for typical navigation scenarios.

Navigating to pages with commands
---------------------------------

The Okra App Framework includes a **GetNavigateToCommand(...)** extension method on the **INavigationBase** interface that returns a
command that will navigate to the specified page. An optional parameter may also be provided. In general you initialize the navigation
commands as follows,

{% highlight c# %}
[ViewModelExport("MyPageName")]
public class MyViewModel : MyViewModelBase
{
    private readonly INavigationBase navigationManager;

    [ImportingConstructor]
    public MyViewModel(INavigationContext navigationContext)
    {
        this.navigationManager = navigationContext.GetCurrent();
        this.GoToPage2Command  = navigationManager.GetNavigateToCommand("Page 2");
    }

    public ICommand GoToPage2Command { get; private set; }
}
{% endhighlight %}

Note that the **GoToPage2Command** is defined as a public property, and initialized in the view-model constructor from the provided navigation
manager. You would then bind the button to this command in your XAML as follows,

{% highlight c# %}
<Button Command="{Binding GoToPage2Command}"/>
{% endhighlight %}

Navigating backwards with commands
----------------------------------

In addition to navigating forward to new pages in the navigation stack, the Okra App Framework provides a **GetGoBackCommand()** extension method
on the **INavigationBase** interface. This can be used in exactly the same way as **GetNavigateToCommand(...)** and will commonly be bound to the
back button of the page. Note also that the resulting command implements **CanExecute**, so will automatically disable any bound buttons if the
user is at the start of the navigation stack.

{% highlight c# %}
[ViewModelExport("MyPageName")]
public class MyViewModel : MyViewModelBase
{
    private readonly INavigationBase navigationManager;

    [ImportingConstructor]
    public MyViewModel(INavigationContext navigationContext)
    {
        this.navigationManager = navigationContext.GetCurrent();
        this.GoBackCommand = navigationManager.GetGoBackCommand();
    }

    public ICommand GoBackCommand { get; private set; }
}
{% endhighlight %}