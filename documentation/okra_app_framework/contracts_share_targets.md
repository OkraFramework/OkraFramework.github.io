---
layout: documentation
title: Implementing a share target
---

Implementing a share target
===========================

Whilst Windows 8.1 applications are sandboxed from other applications, the operating system provides the 'Share' charm as a mechanism to transfer data between applications. There are two players in this interaction, the share source (with the data that is to be shared) and the share target (which accepts and processes the data), both of which are easy to implement using the Okra App Framework.

Detailed below are the steps required to implement a share target. Implementing a share source is discussed [here](contracts_sharing_with_other_applications.html).

Creating a share target page for you application
------------------------------------------------

The [Okra App Framework Visual Studio extension](getting_started_downloading.html) provides a dedicated
template for creating share targets. From the Visual Studio **Add New Item** dialog select the
**Share Target Contract (MVVM)** item. This will create a default XAML file along
with an associated view-model, as well as update the application manifest to enable sharing. You may
wish to further modify the application manifest to configure the types of data that you can share.

Whilst the resulting view-model is fairly complex, all that is required to enable basic sharing
support is to implement the missing code in the **Share()** method. In this method you can access the
shared data via the **this._shareOperation.Data** property as well as any properties of the view-model that
are bound to the UI such as the **this.Comment** property.

{% highlight c# %}
public async void Share()
{
    ...

    string text = await _shareOperation.Data.GetTextAsync();
    string[] comments = string.IsNullOrEmpty(Comment) ? new string[] { } : new string[] { Comment };

    await todoDataStore.AddTodoItemAsync(text, comments);

    ...
}
{% endhighlight %}

Updating the application bootstrapper
-------------------------------------

One other task is required to enable your application as a share target, and that is to import an **Okra.Sharing.IShareTargetManager** as part of you application bootstrapper and configure the
page name for the share target UI.

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
    [Import]
    public Okra.Sharing.IShareTargetManager ShareTargetManager { get; set; }

    ...

    protected override void SetupServices()
    {
        ...

        ShareTargetManager.ShareTargetPageName = "ShareTarget";
    }
}
{% endhighlight %}