Swashbuckle.OData 1.1.0-alpha
=========

[![Build status](https://ci.appveyor.com/api/projects/status/lppv9403dgwrntpa?svg=true)](https://ci.appveyor.com/project/rbeauchamp/swashbuckle-odata)

Extends Swashbuckle with WebApi OData v4 support!

Implements a custom <code>ISwaggerProvider</code> that converts an <code>IEdmModel</code> to a <code>SwaggerDocument</code>.

Try it out at http://swashbuckleodata.azurewebsites.net/swagger

## Getting Started ##

* Install [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)

* Install the Swashbuckle.OData NuGet package:

<code>Install-Package Swashbuckle.OData -Pre</code>

* Update your <code>SwaggerConfig</code> to accept an <code>IEdmModel</code>:
```csharp
//[assembly: PreApplicationStartMethod(typeof(SwaggerConfig), "Register")]

namespace Swashbuckle.OData
{
    public class SwaggerConfig
    {
        public static void Register(IEdmModel edmModel)
        {
```

* In your <code>SwaggerConfig</code> configure the custom provider:
```csharp
// Wrap the default SwaggerGenerator with additional behavior (e.g. caching) or provide an
// alternative implementation for ISwaggerProvider with the CustomProvider option.
//
c.CustomProvider(defaultProvider => new ODataSwaggerProvider(edmModel));
```

* When you build your OData <code>IEdmModel</code>, pass it to <code>SwaggerConfig</code> during registration. For example:
```csharp
public static void Register(HttpConfiguration config)
{
    var builder = new ODataConventionModelBuilder();
    var edmModel = builder.GetEdmModel();
    config.MapODataServiceRoute("odata", "odata", edmModel);

    SwaggerConfig.Register(edmModel);
}
```
Note that, currently, the <code>ODataSwaggerProvider</code> assumes an ODataServiceRoute of "odata".
