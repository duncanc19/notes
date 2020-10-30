# Web API - ASP.NET

HTTP Protocol - 4 main calls
Create - POST - create a new record or insert new value
Read - GET - get information, when you enter an URL, that's a GET command
Update - PUT - modify a record which already exists
Delete - DELETE

```
dotnet new webapi
set up template api with example controller

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

        // // GET api/values/5
        // [HttpGet("{id}")]
        // public ActionResult<string> Get(int id)
        // {
        //     return "value";
        // }

        // // POST api/values
        // [HttpPost]
        // public string Post([FromBody] string value)
        // {
        //     return "hello";
        // }

        // // PUT api/values/5
        // [HttpPut("{id}")]
        // public void Put(int id, [FromBody] string value)
        // {
        // }

        // // DELETE api/values/5
        // [HttpDelete("{id}")]
        // public void Delete(int id)
        // {
        // }
    }
}
```
