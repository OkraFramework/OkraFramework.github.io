---
layout: documentation
title: Adding the Okra App Framework to an Existing Project
---

Adding the Okra App Framework to an Existing Project
====================================================

If you wish to add the Okra App Framework to an existing Windows Store project then the steps required are,

1. Download the Okra App Framework and add the references to your project (downloading via NuGet is recommended)
2. Add a new 'AppBootstrapper.cs' file to your project with the following code,

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
}
{% endhighlight %}


3. Clean up your 'App.xaml.cs' file so that it looks like,

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


4. Change the 'App.xaml' definition to derive from OkraApplication (note that you can keep any existing resources and namespace definitions if desired),

{% highlight c# %}
<okra:OkraApplication
    ...
    xmlns:okra="using:Okra">

    ...

</okra:OkraApplication>
{% endhighlight %}