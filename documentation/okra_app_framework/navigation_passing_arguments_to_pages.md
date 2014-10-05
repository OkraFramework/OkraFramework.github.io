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
void NavigateTo(string pageName);
void NavigateTo(string pageName, object arguments);
{% endhighlight %}

Note that whilst the 'arguments' parameter can be of any type, there are a number of restrictions that should be
observed in practice,

* The type should be serializable via a DataContractSerializer
* The type should not rely on a reference to any particular object (for example if the value is from a shared cache it may be better to pass
  the key rather than the actual object)

Activating destination pages with arguments
-------------------------------------------

To enable a page to accept arguments the destination view-model should implement the **IActivatable** interface.

{% highlight c# %}
public interface IActivatable 
{ 
    void Activate(PageInfo pageInfo); 
    void SaveState(PageInfo pageInfo); 
}
{% endhighlight %}

Upon navigation the Okra App Framework will create the page and view-model as usual, followed by a call the **Activate(...)** method.
This method will only be called once and prior to display of the view. This method is passed a PageInfo object that contains the information
required to describe the destination page. By calling the **PageInfo.GetArguments<T>()** method you can obtain the arguments passed from the source
page. Note that the type argument 'T' should match the type specified in the **NavigateTo(...)** method.

Note that if session state storage is not being implemented then the **SaveState(...)** method can be empty.

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

The corresponding "ViewPhoto" view-model is written as,

{% highlight c# %}
[ViewModelExport("ViewPhoto")]
public class ViewPhotoViewModel : MyViewModelBase, IActivatable
{
    ...
 
    public void Activate(PageInfo pageInfo)
    {
        string photoId = pageInfo.GetArguments<string>();
        this.Photo = GetPhoto(photoId);
    }
 
    public void SaveState(PageInfo pageInfo)
    {
    }

    private Photo GetPhoto(string photoId)
    {
        ...
    }
}
{% endhighlight %}