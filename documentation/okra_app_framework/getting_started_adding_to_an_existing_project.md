---
layout: documentation
title: Adding the Okra App Framework to an Existing Project
---

Adding the Okra App Framework to an Existing Project
====================================================

If you wish to add the Okra App Framework to an existing Windows Store project then the steps required are,

<ol>
<li>Download the Okra App Framework and add the references to your project ([downloading via NuGet is recommended](getting_started_downloading.html#bookmark-nuget))</li>
<li>Add a new 'AppBootstrapper.cs' file to your project with the following code,</li>

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
}
{% endhighlight %}


<li>Clean up your 'App.xaml.cs' file so that it looks like,</li>

{% highlight c# %}
using Okra;

namespace MyOkraProject
{
    sealed partial class App : OkraApplication
    {
        public App()
            : base(new AppBootstrapper())
        {
            this.InitializeComponent();
        }
    }
}
{% endhighlight %}


<li>Change the 'App.xaml' definition to derive from OkraApplication (note that you can keep any existing resources and namespace definitions if desired),</li>

{% highlight c# %}
<okra:OkraApplication
    ...
    xmlns:okra="using:Okra">

    ...

</okra:OkraApplication>
{% endhighlight %}
</ol>