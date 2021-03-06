# Web API - ASP.NET

HTTP Protocol - 4 main calls
Create - POST - create a new record or insert new value
Read - GET - get information, when you enter an URL, that's a GET command
Update - PUT - modify a record which already exists
Delete - DELETE

```
dotnet new webapi
set up template api with example controller

dotnet run if in project repo or
dotnet run --project(-p) projectname
run project on local host
```

### How the route is built

The Route line specifies the route of the controller, with [controller] making it the class name of the controller(without controller)

api/[controller] is the standard way of having a route but you can change api to something else - but it's good for making it clear it's an api

```c#
namespace HRApp.API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class HomeController : ControllerBase
    {
        // GET api/home
        [HttpGet]
        public ActionResult<string> Get()
        {
            return "This is the home page! Isn't it great!";
        }

        // POST api/home
        [HttpPost]
        public ActionResult<string> Post()
        {
            return "Hello there";
        }
  ```
  
 ### Example PUT and DELETE commands
 ```cs
        PUT api/values/5
        [HttpPut("{id}")]
        public void Put(int id, [FromBody] string value)
        {
        }

        DELETE api/values/5
        [HttpDelete("{id}")]
        public void Delete(int id)
        {
        }
    }
}
```

GET data is always visible in the URL, you should use POSTs when you want to send secure information and if you want to pass a lot of information in.


### People example

In Models folder, where there are classes for data types:
```cs
namespace apis.Models {
    public class Person {
    public int Id { get; set; } = 0;
    public string FirstName { get; set; } = "";
    public string LastName { get; set; } = "";
    }
}
```

In Controllers folder, where the calls are made:
```cs
namespace apis.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class PeopleController : ControllerBase
    {
        List<Person> people = new List<Person>();


        public PeopleController()
        {
            people.Add(new Person { FirstName = "Bob", LastName = "Bobson", Id = 1 });
            people.Add(new Person { FirstName = "Bill", LastName = "Bobson", Id = 2 });
            people.Add(new Person { FirstName = "Brenda", LastName = "Bobson", Id = 3 });
        }

        [HttpGet]
        public List<Person> Get()
        {
            return people;
        }

        [HttpGet("{id}")]
        public Person GetPerson(int id)
        {
            return people.Where(x => x.Id == id).FirstOrDefault();
        }

        [HttpPost]
        public void Post(Person value)
        {
            people.Add(value);
        }
        
        [Route("firstnames")]
        [HttpGet]
        public List<string> GetFirstNames()
        {
            List<string> output = new List<string>();
            foreach (var p in people)
            {
                output.Add(p.FirstName);
            }
            return output;
        }
    }
}
```

For the bottom GET call which returns names, specified route gets added to the route of the class - api/people/firstnames

Works the same way for GETs as putting [HttpGet("firstnames")]

Passing in information through the URL:

```cs
        [HttpGet("{userId:int}/{age:int}")]
        public string GetIdAndAge(int userId, int age)
        {
            
            return $"Your ID is {userId} and you are {age}";
        }
```
Access through https://localhost:5001/api/people/1/32


The HTTP verb attributes([HttpGet] etc) allows you to change the names of your HTTP request methods so they don't have to include Get/Post at the start of the method name.

Example:
```cs
[HttpGet]
      public string SayHello()
```

### Return types

void
IHttpActionResult - place what you want to return inside the HTTP action, in the return type you can put it in <> brackets

Example:
```cs
[HttpGet]
public IHttpActionResult MyGetAction(MyRequest req) {
  var data = GetData();
  
  if (data == null)
    return InternalServerError(); // 500
  if (!ModelState.IsValid)
    return BadRequest(ModelState); // 400
  return Ok(data); // 200
  
[HttpPost]
        public ActionResult<User> Post([FromBody] UserInfo info)
        {
            Guid id = Guid.NewGuid();
            string username = info.GenerateUsername();
            Login login = new Login { Username = username, Password = "ABC" };
            User user = new User (login, info, id);
            return Ok(user);
        }
```
Http Response Exception can be used so you can have strongly typed return values but this isn't great practice, throwing errors can cause problems. 

```cs
[HttpPost]
public List<Tour> SearchTours([FromBody] TourSearchRequestDto request)
{
  if (request.MinPrice > request.MaxPrice)
    throw new HttpResponseException(new HttpResponseMessage
    {
      StatusCode = HttpStatusCode.BadRequest,
      Content = new StringContent("MinPrice must be less than MaxPrice")
    });
```

ActionResult Helper Methods:
```cs
BadRequest() // 400
Conflict()
Content()
Created()
InternalServerError()
NotFound()
Ok() // 200
Unauthorized()
StatusCode()
```

### Validate Models

Check that the information passed in by a request is valid using the ModelState

Example where two fields are required:
```cs
public class AuthorizeRequestDto
{
  [Required]
  [MinLegth(32), MaxLength(32)]
  public string AppToken { get; set; }
  
  [Required]
  [MinLegth(32), MaxLength(32)]
  public string AppSecret { get; set; }
}

public class AuthorizeController : ApiController
{
  public IHttpActionResult Post(AuthorizeRequestDto request)
  {
    if (!ModelState.IsValid)
      return BadRequest(ModelState);
      
    return Ok();
```
Validation attributes
```cs
[Compare()]             [CreditCard()]
[DataType()]            [EmailAddress()]
[MaxLength()]           [MinLength()]
[Phone()]               [Range()]
[RegularExpression()]   [Required()]
[Url()]                 [ValidationAtrribute()]
```

### Attribute Routing

Sets up the routes for requests right above them, instead of having to set the routes up in the startup.cs file

```cs
[Route("api/tour/{id:int}")] // specify type of input with :int, {} makes it dynamic
public Tour Get(int id)
{
  var item = _context.Tours
    .FirstOrDefault(i => i.TourId == id);
  return item;
}
```

You can specify the route above the class like the first example here or with RoutePrefix, \[controller] assigns the controller name: 
```cs
[ApiController]
[Route("api/[controller]")]
public class PeopleController : ControllerBase

RoutePrefix("api/orders")
```
You can escape a route prefix on a request by using the tilda ~
```cs
[Route("~api/customers/{customerId:int}/orders")]
```

Route Constraints
```cs
alpha           bool
datetime        decimal
double          float
guid            int
length          long
max             maxlength
min             minlength
range           regex
```

### Models and Data Transfer Objects

Data Transfer Objects are simple objects which only contain the data that we need.

You can have a DTOs folder and then use a DTO as a return type instead of a whole object if there is information in an object you don't want to pass. In this example, a tour object also contains a note field which is not intended for users, so using a Dto can stop this information from being shared.

```cs
var query = _context.Tours
  .Select(i => new TourDto {
    Name = i.Name, Price = i.Price, Description = i.Description
  });
```

### CRUD operations - Create, Read, Update, Delete with Entity Framework

Select entity framework controller in VS Code or add using statements for it:
```cs
using System.Data.Entity;
using System.Data.Entity.Infrastructure;

[ResponseType(typeof(Reservation))]
public async Task<IHttpActionResult> PostReservation(Reservation reservation)
{
  if (!ModelState.IsValid)
  {
    return BadRequest(ModelState);
  }
  
  db.Reservations.Add(reservation);
  await db.SaveChangesAsync();
  return CreatedAtRoute("DefaultApi", new { id = reservation.ReservationId }, reservation);
}
```

### Json.NET Settings

You can change your serialization settings with Json.net. DateTime is an important type as you can select how dates are stored - Local(Your server's time), UTC(Greenwich Mean Time).

You can set the formatting of your API to return data with indentation:
```cs
var json = config.Formatters.JsonFormatter.SerializerSettings;
json.Formatting = Formatting.Indented;
```

ReferenceLoopHandling

When converting to Json, some objects may have references back to their parents or to objects which contain a reference to the original object, which could lead to an infinite loop. To prevent problems like that we need to tell them to stop looping. You can throw an error, ignore or continue to serialize. Ignore means that it will move on from objects that it has already serialized.
```cs
var json = config.Formatters.JsonFormatter.SerializerSettings;
json.ReferenceLoopHandling = ReferenceLoopHandling.Ignore;
```

### Error Handling

#### Exception Filters

Exception filters are a reuseable way of handling errors, meaning you can avoid error handling code duplication. You can specify where the error handling is applied by adding the class as an attribute to a method or class, or adding it to config for the whole project.
```cs
using System.Web.Http.Filters;
using System.Net.Http;

public class DbUpdateExceptionFilterAttribute : ExceptionFilterAttribute
{
  public override void OnException(HttpActionExecutedContent context)
  {
    if (!(context.Exception is DbUpdateException)) return;
    
    var sqlException = context.Exception?.InnerException?.InnerException as SqlException;
    if (sqlException?.Number == 2627) // number for violation of unique constraint
      context.Response= new HttpResponseMessage(HttpStatusCode.Conflict);
      
    context.Response = new HttpResponseMessage(HttpStatusCode.InternalServerError);
  }
}

// You can then call this filter which will run when an exception is thrown in a method/class by putting it as an attribute above the method/class:
[DbUpdateExceptionFilter]
// You can add it to all of the controllers by adding it to the config in the Startup.cs file
config.Filters.Add(new DbUpdateExceptionFilterAttribute());
```

#### Exception Loggers

You can have an exception logger in a Loggers folder, which will log any exceptions which are raised during API calls. You override the Log method of the ExceptionLogger base class to change the standard way of logging exceptions. The default log method doesn't give any kind of response.

```cs
public class UnhandledExceptionLogger : ExceptionLogger
{
  public override void Log(ExceptionLoggerContext context)
  {
    var log = context.Exception.Message;
    Debug.WriteLine(log);
    // base.Log(context);  calling the parent log method
  }
}
```
 You can change to your logger in the Startup.cs file:
 ```cs
 config.Services.Replace(typeof(ExceptionLogger), new UnhandledExceptionLogger());
```

#### Global Exception Handler

### Adding Documentation To An API

You can add in XML comments which can be displayed for users of your API. Visual Studio can generate the XML for these comments.

```cs
/// <summary>
/// Gets a list of all tours
/// </summary>
/// <param name="freeOnly">Show free tours only?</param>
/// <returns>List of all matching tours</returns>
[HttpGet]
public List<TourDto> GetAllTours([FromUri]bool freeOnly = false)
```
Add the Microsoft.AspNet.WebApi.HelpPage package to your project, which will create views and a controller for your help pages. Replace GlobalConfiguration.Configuration calls anywhere in your project with Startup.HttpConfiguration.


### Security

#### Authorize and AllowAnonymous Attribute

The authorize attribute is an easy way to enforce authorisation before passing data, you can put it above a method or class like other attributes. Automatically you'll get a 401 unauthorized response and a message saying you don't have authorization.

```cs
[Authorize]
[AllowAnonymous]
```
AllowAnonymous works as an exception to a class which has an authorize attribute, allowing people to access this model without needing authentication.

#### Setting User Principal
```cs
internal class AutoAuthenticationHandler : DelegatingHandler
{
  
}
```
### Json Web Tokens

A security token for the web in Json format. It contains three parts:

A header - to give some basic metadata about the token, like the hashing algorithm used to create the signature

Payload - the body of the token, containing all the information 

Signature - makes the token secure based on a secret code which is used to convert the data into a hash

First you submit credentials(i.e. username and password) to an unsecured endpoint in the API, which will verify your data and then it will issue a token in the response body. For all subsequent requests to the API, the token will be sent in the authorization header.

Authorization: Bearer <Token>
    
Add the JWT package to the project with Nuget - Microsoft.Owin.Security.Jwt

Add a method to the Startup.cs class

```cs
public void ConfigureJwt(IAppBuilder app)
{
    app.UseJwtBearerAuthentication(new JwtBearerAuthenticationOptions {
        AuthenticationMode = AuthenticationMode.Active,
        AllowedAudiences = new [] { GlobalConfig.Audience },
        IssuerSecurityKeyProviders = new IIssuerSecurityKeyProvider[]
        {
            new SymmetricKeyIssuerSecurityKeyProvider(GlobalConfig.Audience, GlobalConfig.Secret);
        }
    });
}
