---
layout: documentation
title: Passing arguments to pages
---

Passing arguments to pages
==========================

When navigating between pages in a Windows Store application you will often wish to pass some data to the destination page. For example
a "Browse Photos" page in a photo browsing application would need to pass the selected photo to a "View Photo" page.

Passing arguments from the source page
--------------------------------------

To support the passing of arguments upon navigation the **NavigateTo(...)** method includes an overload for this purpose,

{% highlight c# %}
public interface INavigationBase
{
    void NavigateTo(string pageName);
    void NavigateTo(string pageName, object arguments);

    ...
}
{% endhighlight %}

All that is required is to supply the argument to the **NavigateTo(...)** method and the Okra App Framework will ensure that this is passed to
the destination page or view-model. The 'arguments' parameter can be of any type, however there are a number of restrictions that should be
observed in practice,

* The type should be serializable via a DataContractSerializer
* The type should not rely on a reference to any particular object (for example if the value is from a shared cache it may be better to pass
  the key rather than the actual object)

Activating destination pages with arguments
-------------------------------------------

To enable a page to accept arguments the destination view-model should implement the **IActivatable<TArguments, TState>** interface. The type of
the argument passed to the page is defined by *TArguments* and should match the type passed from the source page. Additionally, *TState* is used
for handling storage of session state, and if not required should be set to 'string'.

{% highlight c# %}
public interface IActivatable<TArguments, TState>
{
    void Activate(TArguments arguments, TState state);
    TState SaveState();
}
{% endhighlight %}

Upon navigation the Okra App Framework will create the new page and view-model as usual, followed by a call the **Activate(...)** method passing in
the argument passed from the source page. This method will only be called once and prior to display of the view.

Note that if session state storage is not being implemented then the **SaveState()** method should just return null.

Example code
------------

For example in our hypothetical photo browsing application the "BrowsePhotos" view-model would be implemented as shown below. Note that we pass the
photo ID rather than the photo object itself.

{% highlight c# %}
[ViewModelExport("BrowsePhotos")]
public class BrowsePhotosViewModel : MyViewModelBase
{
    private readonly INavigationBase navigationManager;
 
    [ImportingConstructor]
    public BrowsePhotosViewModel(INavigationContext navigationContext)
    {
        this.navigationManager = navigationContext.GetCurrent();
    }

    ...
 
    public void ViewPhoto(Photo photo)
    {
        navigationManager.NavigateTo("ViewPhoto", photo.PhotoId);
    }
}
{% endhighlight %}

The corresponding "ViewPhoto" view-model would be written as,

{% highlight c# %}
[ViewModelExport("ViewPhoto")]
public class ViewPhotoViewModel : MyViewModelBase, IActivatable<string, string>
{
    ...
 
    public void Activate(string arguments, string state)
    {
        this.Photo = GetPhoto(arguments);
    }
 
    public string SaveState()
    {
        return null;
    }

    private Photo GetPhoto(string photoId)
    {
        ...
    }
}
{% endhighlight %}