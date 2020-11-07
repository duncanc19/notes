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

