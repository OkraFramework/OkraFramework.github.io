---
layout: documentation
title: Sharing items with other applications
---

Sharing items with other applications
=====================================

Whilst Windows 8.1 applications are sandboxed from other applications, the operating system provides the 'Share' charm as a mechanism to transfer data between applications. There are two players in this interaction, the share source (with the data that is to be shared) and the share target (which accepts and processes the data), both of which are easy to implement using the Okra App Framework.

Detailed below are the steps required to implement a share source. Implementing a share target is discussed [here](contracts_share_targets.html).

Marking a view-model as a share source
--------------------------------------

Since the data to share is associated with the user context, it is the responsibility of the view-model to specify what data is available. For example in a photo browsing application, the photo detail page could share the current photo, whilst an album page may share a collection of the currently selected photos.

To mark a view-model as a share source it implements the **Okra.Sharing.ISharable** interface.

{% highlight c# %}
public interface IShareable 
{ 
    Task ShareRequested(IShareRequest shareRequest); 
} 
{% endhighlight %}

A single asynchronous method is required to be implemented, **ShareRequested(...)**, where the view-model should add information and data about the item to share to the provided **IShareRequest**. Note that the **IShareRequest** interface mirrors closely the **Windows.ApplicationModel.DataTransfer** class, and much of the Windows documentation can be followed from this point onwards (for example [Quickstart: Sharing content](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh871368.aspx)).

Set metadata using the **shareRequest.Data.Properties.Xxx** properties, and provide the data to share using the **shareRequest.Data.SetXxx** methods. Note that you should provide as many data formats as possible to maximise the chance of matching share targets. If generating the data to share requires network or file access you can provide a callback using a **shareRequest.Data.SetAsyncXxx** method that will only be called if the specified data type is requested by the target application.

{% highlight c# %}
public class MyViewModel : ViewModelBase, IShareable
{
    ...

    public Task ShareRequested(IShareRequest shareRequest) 
    { 
        shareRequest.Data.Properties.Title = string.Format("Todo Item: {0}", TodoItem.Title);
        shareRequest.Data.SetText(TodoItem.Description);
        return Task.FromResult(false); 
    }
}
{% endhighlight %}

Enabling sharing for you application
------------------------------------

One other task is required to enable sharing for your application, and that is to import an **Okra.Sharing.IShareSourceManager** as part of you application bootstrapper.

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
    [Import]
    public Okra.Sharing.IShareSourceManager ShareSourceManager { get; set; }

    ...
}
{% endhighlight %}