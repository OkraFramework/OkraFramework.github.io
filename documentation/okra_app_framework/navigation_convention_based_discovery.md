---
layout: documentation
title: Convention based page and view-model discovery
---

Convention based page and view-model discovery
==============================================

By default the Okra App Framework uses attributes to annotate the pages and view-models for discovery ([see here for more information](navigation_defining_pages_and_viewmodels.html)).
Often however there will be a common naming pattern throughout the application. In a convention based approach you no longer need to apply attributes to classes. Instead
they are automatically discovered based on this common naming system. For example, all pages may be named **XxxPage** and view-models named
**XxxViewModel**, where "Xxx" is the associated page name.

<div class="alert alert-info">
	<a href="{{ site.url }}/samples"><b>Note:</b> A sample project demonstrating convention based discovery is available</a>
</div>

Setting up convention based discovery
-------------------------------------

The Okra App Framework supports convention based discovery by configuration of the underlying MEF composition container in your application bootstrapper.
For example to apply the above conventions you would simply override the **GetContainerConfiguration()** method as follows,

{% highlight c# %}
public class AppBootstrapper : OkraBootstrapper
{
    ...

    // *** Overriden base methods ***

    protected override ContainerConfiguration GetContainerConfiguration()
    {
        ConventionBuilder conventionBuilder = new ConventionBuilder();

        conventionBuilder.ForTypesMatching(type => type.FullName.EndsWith("Page"))
                         .Export(builder => builder.AsContractType<object>()
                                                   .AsContractName("OkraPage")
                                                   .AddMetadata("PageName", type => type.Name.Substring(0, type.Name.Length - 4)));

        conventionBuilder.ForTypesMatching(type => type.FullName.EndsWith("ViewModel"))
                         .Export(builder => builder.AsContractType<object>()
                                                   .AsContractName("OkraViewModel")
                                                   .AddMetadata("PageName", type => type.Name.Substring(0, type.Name.Length - 9)));

        return GetOkraContainerConfiguration()
                .WithAssembly(typeof(AppBootstrapper).GetTypeInfo().Assembly, conventionBuilder);
    }
}
{% endhighlight %}

Note the use of lambda expressions to identify all types ending in the words "Page" or "ViewModel", and the Substring expression to extract the page
name from the type name. Also note that the convention based configuration must be appended to the result from a call to the **GetOkraContainerConfiguration()**
method so that the core Okra services are included.

Using convention based discovery
--------------------------------

Once convention based discovery has been configured you can now simply define pages and view-models following the convention.

For example to define a page named "Foo",

{% highlight c# %}
public sealed partial class FooPage : Page
{
    ...
}

public class FooViewModel
{
    ...
}
{% endhighlight %}