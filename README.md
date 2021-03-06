# Pioneer Pagination


## What is Pioneer Pagination?
Pioneer Pagination is an ASP.Net Core Tag Helper that produces a paginated list which is configured to a desired state.
![Pagination Image](http://pioneercode.com/images/github/pioneer-pagination.png "Pagination Image")

## Where can I get it?
Pioneer Pagination is available as a [NuGet package](https://www.nuget.org/packages/Pioneer.Pagination/). 

## How does it work?
1. In your controller, you make a call to ```PaginatedMetaService``` which returns a ```PaginatedMetaModel```.
2. Your ```PaginatedMetaModel``` is then passed to your view so that the Tag Helper can utilize it. 
3. In your view, you bind the ```PaginatedMetaModel``` to an attribute and set a route on another attribute.
4. The Tag Helper generates a paginated list. 

## How do I use it?

For a full working example, clone this repository and run the [Pioneer.Pagination.Example](https://github.com/PioneerCode/pioneer-pagination/tree/master/src/Pioneer.Pagination.Example) project locally.

### Install
To install, run the following command from your package manager console:
```cmd
PM> Install-Package Pioneer.Pagination
```

### Gain access to the PaginatedMetaService service. 

In your Startup.cs class, add ```PaginatedMetaService``` into your dependency injection container.
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<IPaginatedMetaService, PaginatedMetaService>();
}
```

In your controller, add a reference to the ```IPaginatedMetaService``` interface and set it through dependency injection.
```csharp
public class BlogController : Controller
{
    private readonly IPaginatedMetaService _paginatedMetaService;

    public BlogController(IPaginatedMetaService paginatedMetaService)
    {
        _paginatedMetaService = paginatedMetaService;
    }
}
```

### Add PaginatedMetaModel to ViewBag
From your controller, bind the ```PaginatedMetaModel``` to your ```ViewBag```.
```csharp
public ActionResult Index(int page = 1)
{
	// Typically obtained from a service/repository
    var totalNumberInCollection = 100;
	// Typically obtained from configuration
    var itemsPerPage = 5;
    ViewBag.PaginatedMeta = _paginatedMetaService.GetMetaData(totalNumberInCollection, page, itemsPerPage);
    return View("Blog", post);
}
```

### Add Tag Helper to your view
```csharp
<pioneer-pagination info="@ViewBag.PaginatedMeta" route="/blog"></pioneer-pagination>
```

### Configuration

- **previous-page-text**
  - default = next
- **previous-page-text**
  - default = previous

```csharp
<pioneer-pagination info="@ViewBag.PaginatedMeta" route="/blog" previous-page-text="hey" next-page-text="you"></pioneer-pagination>
```

## What about styling?
The markup this produces is based on [Foundation Pagination](http://foundation.zurb.com/sites/docs/pagination.html).  This leaves you one of three options.

1. Use [Foundation](http://foundation.zurb.com/sites/docs/) and it will work out of the box.
2. Map the classes to Bootstrap stylings.
3. Use the starting [CSS](https://github.com/PioneerCode/pioneer-pagination/blob/master/src/Pioneer.Pagination.Example/wwwroot/pioneer.pagination.css) or [sass](https://github.com/PioneerCode/pioneer-pagination/blob/master/src/Pioneer.Pagination.Example/sass/pioneer.pagination.scss) files provided in the example.

## Change Log

### [2.1.0]
- Add configuration for previous and next verbiage - [ryn0](https://github.com/ryn0)

### [2.0.0]
- Migarted to .NET Standard 2.x
- Migarted example project to ASP.NET Core 2.x

### [1.1.2]
- Fixed Next button not being displayed 
	- Next should be displayed up until last page.
	- Added supported UTs. 

### [1.1.0]
- Upgrade to LTS 1.0.3
- Fixed previous and next button indexing
   - Supporting UTs.
