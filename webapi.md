# ASP.NET Core Web API Hints, Tips and Snipits

[Home](index.md)

## Quick links

- [REST API Tutorial](https://restfulapi.net)
- [Swagger OpenAPI](https://swagger.io/specification/)
- [Swachbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)

## Topics To Consider

- StyleCop compliant
- CodeAnalysis compliant
- All code in .NET Standard library
- NuGet package creation
- Application authentication/authorization
- User authentication/authorization
- TLS support
- Configuraiton
- Logging
- Error handling
- API versioning
- Caching (ETag/If-Match/If-None-Match)
- HATEOAS
- Unit testing
- Documentation

## Definitions

| Definition | Descriptio                                    |
|------------|-----------------------------------------------|
| REST       | REpresentational State Transfer               |
| HATEAOS    | Hypermedia As The Engine Of Application State |

## HTTP Verbs

| Verb    | Description                                       |
|---------|---------------------------------------------------|
| GET     | Retrieve a resource                               |
| POST    | Add a new resource                                |
| PUT     | Update an existing resource                       |
| PATCH   | Update an existing resource with a set of changes |
| DELETE  | Remove the existing resource                      |
| OPTIONS | Get options allowed for a resource                |
| HEAD    | Get HTTP headers only                             |

## HTTP Status Codes

| Code | Description        | Usage                  |
|------|--------------------|------------------------|
| 200  | OK                 | General Success        |
| 201  | Created            | Object Created         |
| 202  | Accepted           |                        |
| 302  | Found              |                        |
| 304  | Not Modified       | Use Cached             |
| 307  | Temp Redirect      |                        |
| 308  | Perm Redirect      |                        |
| 400  | Bad Request        | General Client Error   |
| 401  | Not Authorized     | Authentication Failure |
| 403  | Forbidden          | Authorization Failure  |
| 404  | Not Found          | Not Found              |
| 405  | Method Not Allowed |                        |
| 409  | Conflict           |                        |
| 500  | Internal Error     | General Server Error   |

## HTTP Response for REST API

| Verb   | /resources        | /resources/item       |
|--------|-------------------|-----------------------|
| GET    | 200 \{Items}      | 200 \{Item}, 404      |
| POST   | 201 \{Item, Link} | 400                   |
| PUT    | 400               | 200 \{Item}, 404, 400 |
| DELETE | 400               | 200, 404              |

## Content Types

| Type  | MIME Type              |
|-------|------------------------|
| JSON  | application/json       |
| XML   | text/xml               |
| JSONP | application/javascript |
| RSS   | application/xml+rss    |
| ATOM  | application/xml+atom   |

## StyleCop/Code Analysis Compliance

```
    <PackageReference Include="Microsoft.CodeAnalysis.FxCopAnalyzers" Version="2.6.1" PrivateAssets="all" />
    <PackageReference Include="StyleCop.Analyzers" Version="1.0.2" PrivateAssets="all" />
```

## ASP.NET Core MVC

```
    <PackageReference Include="Microsoft.AspNetCore" Version="2.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="2.1.1" />
```

```
    using Microsoft.AspNetCore.Builder;
    using Microsoft.Extensions.DependencyInjection;

    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc();
        }

        public void Configure(IApplicationBuilder app)
        {
            app.UseMvc();
        }
    }
```

Sample controller from https://code-maze.com/unit-testing-aspnetcore-web-api/:
```
[Route("api/[controller]")]
[ApiController]
public class ShoppingCartController : ControllerBase
{
    private readonly IShoppingCartService _service;
 
    public ShoppingCartController(IShoppingCartService service)
    {
        _service = service;
    }
 
    // GET api/shoppingcart
    [HttpGet]
    public ActionResult<IEnumerable<ShoppingItem>> Get()
    {
        var items = _service.GetAllItems();
        return Ok(items);
    }
 
    // GET api/shoppingcart/5
    [HttpGet("{id}")]
    public ActionResult<ShoppingItem> Get(Guid id)
    {
        var item = _service.GetById(id);
 
        if (item == null)
        {
            return NotFound();
        }
 
        return Ok(item);
    }
 
    // POST api/shoppingcart
    [HttpPost]
    public ActionResult Post([FromBody] ShoppingItem value)
    {
        if (!ModelState.IsValid)
        {
            return BadRequest(ModelState);
        }
 
        var item = _service.Add(value);
        return CreatedAtAction("Get", new { id = item.Id }, item);
    }
 
    // DELETE api/shoppingcart/5
    [HttpDelete("{id}")]
    public ActionResult Remove(Guid id)
    {
        var existingItem = _service.GetById(id);
 
        if (existingItem == null)
        {
            return NotFound();
        }
 
        _service.Remove(id);
        return Ok();
    }
}
```

## Logging

```
    using Microsoft.Extensions.Logging;

    public class Startup
    {
        public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
        {
            loggerFactory.AddDebug();
        }
    }
```

```
    using Microsoft.Extensions.Logging;

    public class MyController : Controller
    {
        private ILogger<MyController> logger;

        public MyController(ILogger<MyController> logger)
        {
            this.logger = logger;
        }

        public IActionResult Get()
        {
            this.logger.LogInformation("Hello");
        }
    }
```

## Entity Model Mapping

```
    <PackageReference Include="AutoMapper.Extensions.Microsoft.DependencyInjection" Version="5.0.1" />
```

```
    using AutoMapper;

    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddAutoMapper();
        }
    }
```

```
    using AutoMapper;

    public class MyMappingProfile : Profile
    {
        public MyMappingProfile()
        {
            CreateMap<Entity, Model>();
        }
    }
```

```
    using AutoMapper;

    public class MyController : Controller
    {
        private IMapper mapper;

        public MyController(IMapper mapper)
        {
            this.mapper = mapper;
        }

        public IActionResult Get()
        {
            return Ok(this.mapper.Map<IEnumerable<Model>>(entity));
        }
    }
```

## Model State and Validation

```
    using System.ComponentModel.DataAnnotations;

    public class Model
    {
        [Required]
        public string Name { get; set; }
    }
```

### Via Controller Method

```
    [HttpPost]
    public async Task<IActionResult> Post([FromBody]Model model)
    {
        if (!ModelState.IsValid)
        {
            return BadRequest(ModelState);
        }

        ...
    }
```

### Via ActionFilterAttribute

```
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.AspNetCore.Mvc.Filters;

    public class ValidateModelAttribute : ActionFilterAttribute
    {
        public override void OnActionExecuting(ActionExecutingContext context)
        {
            base.OnActionExecuting(context);

            if (!context.ModelState.IsValid)
            {
                context.Result = new BadRequestObjectResult(context.ModelState);
            }
        }
    }
```

```
    [ValidateModel]
    public class MyController : Controller
    {
    }
```

## Jason Web Tokens (JWT)

https://jwt.io

```
    using System.IdentityModel.Tokens.Jwt;

    [Route("api/[controller]")]
    public class AuthenticationController : Controller
    {
        [HttpPost]
        public async Task<IActionResult> Token([FromBody]UserCredentials model)
        {
            var user = this.UserManager.FindByNameAsync(model.UserName);

            if (user != null)
            {
                if (this.hasher.VerifyHashedPassword(user, user.PasswordHash, model.Password) == PasswordVerificationResult.Success)
                {
                    var claims = new[]
                    {
                        // minimum required
                        new Claim(JwtRegisteredClaimNames.Sub, user.UserName),
                        new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
                        // additional information
                        new Claim(JwtRegisteredClaimNames.GivenName, user.FirstName),
                        new Claim(JwtRegisteredClaimNames.FamilyName, user.LastName),
                        new Claim(JwtRegisteredClaimNames.Email, user.Email)
                    };

                    var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("KEY OBTAINED FROM CONFIGURATION"));
                    var creds = new SigningCredentails(key, SecurityAlgorithms.HmacSha256);

                    var token = new JwtSecurityToken(
                        issuer: "http://mywebsite.com",
                        audience: "http://mywebsite.com",
                        claims: claims,
                        expires: DateTime.UtcNow.AddMinutes(15),
                        signingCredentails: creds
                        );

                    return Ok(new
                    {
                        token = new JwtSecurityTokenHandler().WriteToken(token),
                        expiration = token.ValidTo
                    });
                }
            }

            return this.BadRequest("Authentication failure.");
        }
    }
```

```
    using Microsoft.AspNetCore.Authentication.JwtBearer;

    public class Startup
    {
        public void Configure(IApplicationBuilder app)
        {
            app.UseJwtBearerAuthentication(new JwtBearerOptions
            {
                AutomaticAuthenticate = true,
                AutomaticChallenge = true,
                TokenValidationParameters = new TokenValidationParameters
                {
                    ValidIssuer = "http://mywebsite.com",
                    ValidAudience = "http://mywebsite.com",
                    ValidateIssuerSigningKey = true,
                    IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("KEY OBTAINED FROM CONFIGURATION"")),
                    ValidateLifetime = true
                }
            });
        }
    }
```

```
    [Authorize]
    public class MyController : Controller
    {
        [HttpGet("/api/test")]
        public IActionResult Get()
        {
            return Ok($"Hello {this.User.Identity.Name}");
        }
    }
```

```
GET /api/test HTTP/1.1
Authorization: bearer {token}
```

## API Versioning

```
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Versioning" Version="2.1.1" />
```

```
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddApiVersioning(config =>
            {
                var reader = new QueryStringOrHeaderApiVersionReader("version");
                reader.HeaderNames.Add("Version");
                reader.HeaderNames.Add("X-Version");

                config.DefaultApiVersion = new ApiVersion(1, 1);
                config.AssumeDefaultVersionWhenUnspecified = true;
                config.ReportApiVersions = true;
                config.ApiVersionReader = reader;
            });
        }
    }
```

```
    [ApiVersion("1.0")]
    [ApiVersion("1.1")]
    public class MyController : Controller
    {
        [HttpGet]
        [MapToApiVersion("1.0")]
        public IActionResult Get()
        {
            return Ok("Hello from version 1.0");
        }

        [HttpGet]
        [MapToApiVersion("1.1")]
        public IActionResult Get11()
        {
            return Ok("Hello from version 1.1");
        }
    }
```

## Get Options

```
    [HttpOptions]
    public IActionResult GetOptions()
    {
        this.Response.Headers.Add("Allow", "GET,OPTIONS,POST,DELETE");
    }
```
