---
layout: documentation
title: State persistence - making your app "always alive"
---

State persistence - making your app "always alive"
==================================================

One of the challenges when writing mobile applications is to manage the application lifecycle. To
preserve battery life and performance, apps may be suspended as the user moves to other applications.
Furthermore, your application may be terminated to free more memory if this is required.

One aspect of making your application feel "always alive" is to ensure that if it is later
relaunched then the user is taken to exactly the same point that they were previously viewing. The Okra
App Framework simplifies this process by persisting the navigation stack in addition to allowing for page
specific state.

Persisting the navigation stack
-------------------------------

By default the navigation stack is not automatically persisted. To enable this you should add the
following code to your **AppBootstrapper.SetupServices()** method.

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
    protected override void SetupServices()
    {
        NavigationManager.NavigationStorageType = NavigationStorageType.Local;
    }
}
{% endhighlight %}

There are three possible NavigationStorageType values,

<table class="table ">
	<thead>
		<tr><th>Value</th><th>Navigation persistence</th></tr>
	</thead>
	<tbody>
		<tr><td>None</td><td>Your navigation state will not be persisted and your application will always launch to your home page</td></tr>
		<tr><td>Local</td><td>Your navigation state will be persisted to the current machine only</td></tr>
		<tr><td>Roaming</td><td>Your navigation state will be persisted between all devices that the user logs into using their Microsoft Account</td></tr>
	</tbody>
</table>

<div class="alert alert-info">
	<b>Note:</b> If you are using the Okra App Framework Visual Studio project templates then the navigation
	storage type is set to 'Local' by default.
</div>

Preserving page state
---------------------
In some cases you may want to go a step further and store some page state. Imagine for example you are writing
a comment in a social networking app, then change application. When you return you should expect
to continue from where you left off. Other things you may want to preserve are selected search
filters, selected items, position in a view, etc.

To allow a page to persist state the view-model should implement the **IActivatable** interface.

{% highlight c# %}
public interface IActivatable 
{ 
    void Activate(PageInfo pageInfo); 
    void SaveState(PageInfo pageInfo); 
}
{% endhighlight %}

When the page is initiated, the **Activate(...)** method is called with any page state encapsulated in a
**PageInfo** object. If this is the first navigation to this page then no state will available, and calls to
the **PageInfo.TryGetState&lt;T&gt;(string key, out T value)** method will return false.

If however the Okra App Framework needs to suspend the page for any reason, the **SaveState(...)** method
will be called. In this case the view-model can persist any state using the
**PageInfo.SetState&lt;T&gt;(string key, T value)** method. When the page is reactivated the
**PageInfo.TryGetState&lt;T&gt;(string key, out T value)** method will return true, with the persisted state
returned as 'value'.

Example code
------------

For example, in our hypothetical social networking application you can comment on the current post.
In this case we may wish to persist a partially written comment when the user switches to another application,
such that if the application is terminated it can be restored when the user returns. In this case the "ViewPost"
view-model has a 'Comment' property that is data-bound to the UI.

{% highlight c# %}
[ViewModelExport("ViewPost")]
public class ViewPostViewModel : MyViewModelBase, IActivatable
{
    public string Comment { ... }

    ...
 
    public void Activate(PageInfo pageInfo)
    {
        ...

        string persistedComment;

        if (pageInfo.TryGetState("Comment", out persistedComment))
            this.Comment = persistedComment;
    }
 
    public void SaveState(PageInfo pageInfo)
    {
        pageInfo.SetState("Comment", this.Comment);
    }
}
{% endhighlight %}