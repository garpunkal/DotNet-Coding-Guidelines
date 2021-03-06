# C# Coding Guidelines & Practices

## Tip #1
✋ Try to avoid the tradional if-else statements like in the following:
````csharp
bool result;
if (condition)
{
   result = true;
}
else
{
   result = false;
}
````
👍 Do use ternary conditional operator (?:) instead:
````csharp
bool result = condition ? true: false;
````
The preceding code is much cleaner, easier to read and understand. On top of that, it's more concise.

## Tip #2
✋ Try to avoid using if statement for null checks like in the following:
```csharp
if (something != null)
{
    if (other != null)
    {
        return whatever;
    }
}
```
👍 Do use null conditional (?.) operator instead:
```csharp
return something?.other?.whatever;
```
The preceding code is also much cleaner and concise.

## Tip #3
✋ Try to avoid complex if-else statements for null checks like in the following:
```csharp
if (something != null)
{
    if (other != null)
    {
        return whatever;
    }
    else 
    {
        return string.empty;
    }
}
else
{
    return string.empty;
}
```
👍 Do use null coalescing (??) operator instead:
```csharp
return something?.other?.whatever ?? string.empty;
```

## Tip #4
✋ Try to avoid using the following code when returning a default value when an object is null:
```csharp
int? number = null;
var n = number.HasValue ? number : 0;
```
👍 Do use null coalescing (??) operator as well:
```csharp
var n = number ?? 0;
```
👍 or alternatively, you could do:
```csharp
var n = number.GetValueOrDefault();
```

## Tip #5
✋ Try to avoid using the equality operator (==) or HasValue for nullable variable check like in the following:
```csharp
int? number = null;

if (number == null)
{
    //do something
}

if (!number.HasValue)
{
    //do something
}
```
👍 While the preceding code is fine, we can still improve that by using the is keyword like in the following:
```csharp
int? number = null;

if (number is null)
{
    //do something
}
```

## Tip #6
✋ Avoid code with braces ({}) for single conditional if statement, for and foreach loops like in the following:
```csharp
if (condition) { action };
```

👍 Always remove the braces:
```csharp
if (condition) action; 

// or better

if (condition) 
    action; 

```

## Tip #7
✋ Try to avoid using multiple if-else statements like in the following:
```csharp
if (condition)
{
   //do something
}
else if(condition)
{
   //do something
}
else if(condition)
{
   //do something
}
else(condition)
{
   //do something else
}
```
👍 Do use switch statements instead:
```csharp
switch(condition)
{
   case 1:
      //do something
      break;
   case 2:
      //do something
      break;
   case 3:
      //do something
      break;
   default:
      //do something else
     break;
}
```
👍 But prefer switch expressions over switch statements where possible like in the following:
```csharp
condition switch
{
    1 => //do something;
    2 => //do something;
    3 => //do something;
    _ => //do something else;
}
```
The preceding code is more concise yet, still easy to read and understand. (Note, only available in C# 8 or later versions) 💡 Exceptions - There are cases that if-else statements would make more sense than using switch statements. For example, if the condition involves different objects and complex conditions.

## Tip #8
👍 Do use the using statement when working with objects that eat resources or implements IDisposable interface:
```csharp
using (MemoryStream stream = new MemoryStream()) 
{
    // do something
}
```
👍 Or prefer to use the new using declaration introduced in C# 8 like in the following:
```csharp
using var stream = new MemoryStream();
// do something
```
The preceding code reduces the number of curly braces in your method, but it can still be seen easily where a resource is disposed. For more information, see: "pattern-based using" and "using declarations"

## Tip #9
✋ Avoid concatenating strings with the + sign/symbol like in the following:
```csharp
string name = "Vianne";
string greetings = "Hello " + name + "!";
```
👍 Use string.Format() method instead:
```csharp
string name = "Vynn";
string greetings = string.Format("Hello {0}!", name);
```
👍 Or prefer using Prefer using string interpolation ($) instead where possible:
```csharp
string name = "Vjor";
string greeting = $"Hello, {name}!";
```
The preceding code is much more concise and readable compared to other approaches.

## Tip #10
✋ Try to avoid string.Format() when formatting simple objects like in the following:
```csharp
var date = DateTime.UtcNow;
string greetings = string.Format("Today is {0}, the time is {1:HH:mm} now.", date.DayOfWeek, date);
```
👍 Use string interpolation instead:
```csharp
var date = DateTime.UtcNow;
string greetings = $"Today is {date.DayOfWeek}, the time is {date:HH:mm} now.");
```
The preceding code is much easier to understand and concise. However, there are certain cases that using the string.Format() would makes more sense. For example, when dealing with complex formatting and data manipulation. So, use your judgement when to apply them in situations.

## Tip #11
✋ Avoid using specific type for complex objects when defining variables like in the following:

```csharp
List<Repository.DataAccessLayer.Whatever> listOfBlah = _repo.DataAccessLayer.GetWhatever();
```

👍 Use the var keyword instead:

```csharp
var listOfBlah = _repo.DataAccessLayer.GetWhatever();
```
👍 Same goes for other local variables:
```csharp
var students = new List<Students>(); 
var memoryStream = new MemoryStream();
var dateUntilProgramExpiry = DateTime.UtcNow; 
```

A suggestion would be to use var where the type is clear, if the type isn't clear, then use the type. 

```csharp
var students = new List<Students>();
Student student = Method.Get(); 
```


## Tip #12
✋ Try to avoid one-liner method implementation with curly braces like in the following:
```csharp
public string Greeter(string name)
{
    return $"Hello {name}!";
}
```
👍 Do use Expression-bodied (=>) implementation instead:
```csharp
public string Greeter(string name) => $"Hello {name}!";
The preceding code is more concise while maintaining readability.
```

## Tip #13
✋ Avoid object initialization like in the following:
```csharp
Person person = new Person();
person.FirstName = "Vianne";
person.LastName = "Durano";
```
👍 Do use object and collection initializers instead:
```csharp
var person = new Person { 
	FirstName = "Vianne",
	LastName = "Durano"
};
```

The preceding code is more natural to read and the intent is clear because the properties are defined within braces.

## Tip #14
✋ Avoid creating a class just to return two simple result sets like in the following:
```csharp
public Person GetName()
{
    var person = new Person
    {
        FirstName = "Vincent",
        LastName = "Durano"
    };
    
    return person;
}
```
👍 Do use Tuples instead where possible:
```csharp
public (string FirstName, string LastName) GetName()
{
    return ("Vincent", "Durano");
}
```
The preceding code is more convenient for accessing objects and manipulating the data set. Tuples replaces the need to create a new class whose sole purpose is to carry around data.

## Tip #15
✋ Try to create an Extention Methods to perform common tasks such as conversion, validationn, formatting, parsing, transformation, you name it. So, instead of doing the following:

```csharp
string dateString = "40/1001/2021";
var isDateValid = DateTime.TryParse(dateString, our var date);

```
The preceding code is perfectly fine and should handle the conversion safely. However, the code is bit lengthy just to do basic conversion. Imagine you have tons of the same code conversion cluttering within the different areas in your project. Your code could turn into a mess or potentially causes you alot of development time overtime.

👍 To prevent that, you should consider creating a helper/utility functions to do common tasks that can be reused across projects. For example, the preceding code can now be converted to following extension:
```csharp
public static class DateExtensions
{
     public static DateTime ToDateTime(this string value)
         => DateTime.TryParse(value, out var result) ? result : default;
}
```
and you will be able to use the extension method like in the following anywhere in your code:
```csharp
var date = "40/1001/2021".ToDateTime();
```
The preceding code makes your code concise, easy to understand and provides convenience.

## Tip #16
✋ Try to avoid the following when initializing private and read-only properties if you are using C# 9:
```csharp
public class PersonManager    
{  
    private readonly ILogger<PersonManager> _logger;  
    private readonly SomeConfiguration _config;  
  
    public PersonManager(ILogger<PersonManager> logger, 
		           SomeConfiguration config)  
    {  
         _logger = whatever;  
         _config = config;  
    }  
} 
```
The preceding code is pretty much common for implementing dependency injection.

👍 With the release of C# 9, We can now let the constructor do the initialization automatically:
```csharp
public class PersonManager    
{  
    public PersonManager(ILogger<PersonManager> _logger, 
		        SomeConfiguration _config)  
    {  
  
    }  
} 
```
The preceding code removes alot of noise in your code when injecting dependencies as you don't need to write private readonly declarations which can make your code cleaner.

In situations where you want to expose one of the fields to be public, you can define and set it in the constructor as what you would normally do. Otherwise, the arguments are marked as private fields.

## Tip #17
✋ Avoid using .NET predefined data types such as Int32, String, Boolean, etc.:
```csharp
String firstName; 
Int32 orderCount; 
Boolean isCompleted; 
```
👍 Do use built-in primitive data types instead:
```csharp
string firstName; 
int orderCount; 
bool isCompleted; 
```
The preceding code is consistent with the Microsoft’s .NET Framework and makes code more natural to read.

## Tip #18
✋ Do not use initials as identifier abbreviations like in the following:
```csharp
private readonly PersonManager _pm;
```
The main reason for this is that it can cause confusion and inconsistency when you have class that might represents the same thing like in the following:
```csharp
private readonly ProductManager _pm;
```
👍 Instead, do choose clarity ver brevity like in the following:
```csharp
private readonly PersonManager _personManager;
private readonly ProductManager _productManager;
```
The preceding code provides more clarity as it clearly suggests what the object is about.

## Tip #19
👍 Do organize namespaces with a clearly defined structure. Generally namespaces should reflect the folder hierarchy within a project. Take a look at the followinng example:
```csharp
namespace ProjectName.App.Web
namespace ProjectName.Services.Common
namespace ProjectName.Services.Api.Payment
namespace ProjectName.Services.Api.Ordering
namespace ProjectName.Services.Worker.Ordering
```
The preceding code suggest good organization of your code within the project, allowing you to navigate between layers easily.

## Tip #20
👍 Do use singular form, noun or noun phrases to name a class:
```csharp
public class Person
{
    //some code
}

public class BusinessLocation
{
    //some code
}

public class DocumentCollection
{
    //some code
}
```
This enables you to easily determine if an object holds a single item value or collection. Imagine, if you have a List<People> vs List<Person>. It's just odd to put plural form names in a List or Collection.
  
## Tip #21
👍 Do use nouns or adjective phrases for Property names as well. When naming boolean properties or variables, you may add the prefix "can", "is", "has", etc. just like in the following:
```csharp
public bool IsActive { get; set; }
public bool CanDelete { get; set; }

//variables
bool hasActiveSessions = false;
bool doesItemExist = true;
```
Adding those suffixes will provide more value to the caller.

## Tip #22
👍 Do use Pascal Casing for Class, Method, Property and Constant variable names:
```csharp
public class ClassName 
{ 
    const int MaxPageSize = 100;
    
    public string PropertyName { get; set; } 
	
    public void MethodName() 
    { 
        //do something
    } 
} 
```
This is so that our code are consistent with the Microsoft .NET Framework.

## Tip #23
👍 Do use Camel Casing for method arguments and local variables:
```csharp
public void MethodName(CreatePersonRequestDto requestDto) 
{ 
    var firstName = requestDto.FirstName; 
} 
```
This is so that our code are consistent with the Microsoft .NET Framework.

## Tip #24
👍 Do use meaningful and self-explanatory names for classes, methods and properties:
```csharp
int daysUntilProgramExpiry;

public List<Person> GetPersonProfileById(long personId)
{
    //do something
}
```
This makes your code easier to read and understand without having you to write (or atleast minimizes) comments of what the code does.

## Tip #25
👍 Do suffix asynchronous methods with the Async word:
```csharp
public async Task<List<Person>> GetPersonProfileByIdAsync(long personId)
{
    //do something
}
```

This enable developers to easily identify synchornous vs asynchronous methods by just looking at the method itself.

## Tip #26
Do prefix interfaces with the capital letter I
```csharp
public interface IPersonManager 
{ 
   //...
} 
```
This is to easily distinguish between an interface and classes. In fact, it's a well known standard for defining interfaces.

## Tip #27
👍 Do prefix global variables and class members with underscores (_):
```csharp
private readonly ILogger<ClassName> _logger;
private long _rowsAffected;
private IEnumerable<Persons> _people;
```
This is to easily differentiate between local and global identifiers/variables.

## Tip #28
👍 Do declare all member variables and fields at the top of a class, with static fields at the very top:
```csharp
private static string _externalIdType;
private readonly ILogger<PersonManager> _logger;
private int _age;
```
This is just a generally accepted practice that prevents the need to hunt for variable declarations.

## Tip #29
👍 Do consider putting all your private methods at the botoom after public methods:
```csharp
public class SomeClass
{
    private void SomePublicMethodA()
    {

    }

    // rest of your public methods here

    private void SomePrivateMethodA()
    {

    }

    private void SomePrivateMethodB()
    {

    }
}
```
Why? same reason for Tip #28.

## Tip #30
✋ Try to avoid grouping your code into regions like in the following:
```csharp
#region Private Members
    private void SomePrivateMethodA()
    {

    }

    private void SomePrivateMethodB()
    {

    }

#endregion
```
The preceding code is a code smell which could potentially make your code grow without you realizing it. I admit that I have used this feature many times to collapse the code within a class. However, I realize that hiding code into regions won't give you any value aside from maximizing your visual view when the region is collapsed. If you are working with a team of developers on a project, chances are, other developers will append their code in there until the code get's bigger and bigger over time. As a good practice, it's always recommended to keep your classes small as possible.

If you have tons of private methods within a class, you could split them into a separate class instead.

## Tip #30
👍 Try to use short-hand names only when they’re generally known:
```csharp
private readonly CreateQuestionDefinitionRequestDto _requestDto;
```
It would be too much to name a variable "createQuestionDefinitionRequestDto" when you know that the variable/parameter is a request object. The same thing applies for FTP, UI, IO, etc. It's perfectly fine to use abbreviation for as long as they're generally known, otherwise it would be counter productive not to do so.

## Tip #31
✋ Avoid underscores (_) in between identifier names:
```csharp
public PersonManager person_Manager;
private long rows_Affected;
private DateTime row_updated_date_time;
```
The reason being is that C# isn't postgres. Seriously, it's to be consistent with the Microsoft .NET Framework convention and makes your code more natural to read. It can also avoid "underline stress" or inability to see underline.

## Tip #32
✋ Do not use SCREAMING CAPS for constants or read-only variables:
```csharp
public static const string EXTERNALIDTYPE = "ABC"; 
public static const string ENVIRONMENT_VARIABLE_NAME = "TEST"; 
```
They just grab too much attention.

## Tip #33
✋ Do not use Hungarian notation or any other type identification in identifiers (except interfaces):
```csharp
int iCounter; 
string strName;
string spCreateUsers; 
OrderingService svcOrdering;
```
Visual Studio code editor already provides helpful tooltips to determine object types. In general, you want to avoid type indicators in the identifier.

## Tip #34
✋ Do not use an "Enum" suffix in enum type names and do not use plural form names for enums.

The following is an example for defining an enum:
```csharp
public enum BeerType
{
  Lager,
  Ale,
  Ipa,
  Porter,
  Pilsner
} 
```
Again, this is to be consistent with the Microsoft .NET framework and avoids type indicators in the identifier.

## Tip #35
👍 Try to use record types for immutable objects. Record types is a new feature introduced in C# 9 where it simplfies your code. For example, the following code:
```csharp
public class Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }

    public Person(string firstName, string lastName)
    {
        FirstName = firstName;
        LastName = lastName;
    }
}
```
can be written in the following way using record:
```csharp
public record Person(string FirstName, string LastName);
```
Using record types will automatically generates the boilerplate code for you and keeping your code concise. Records will be really useful for defining DTOs, Commands or any object that carries immutable data around. For more information about this feature, see: [Record Types](https://devblogs.microsoft.com/dotnet/c-9-0-on-the-record/)

## Tip #36
👍 Use pattern matching. 

This allows you to match your variable to a type and then easily assign to a variable: 
https://docs.microsoft.com/en-us/dotnet/csharp/pattern-matching

```csharp
if (shape is Square s)
    return s.Side * s.Side;
```

It works well in switch statements too: 
```csharp
switch (shape)
{
    case Square s:
        return s.Side * s.Side;
    case Circle c:
        return c.Radius * c.Radius * Math.PI;
    case Rectangle r:
        return r.Height * r.Length;
    default:
        throw new ArgumentException(
            message: "shape is not a recognized shape",
            paramName: nameof(shape));
}
```

## Tip #37
👍 Use when clauses in case expressions

if you want to add extra conditions to the switch statements, you can use 'when' alongside pattern matching.

```csharp
switch (shape)
{
    case Square s when s.Side == 0:
    case Circle c when c.Radius == 0:
        return 0;

    case Square s:
        return s.Side * s.Side;
    case Circle c:
        return c.Radius * c.Radius * Math.PI;
    default:
        throw new ArgumentException(
            message: "shape is not a recognized shape",
            paramName: nameof(shape));
}
```

## Tip #38
👍 discarding

You can discard a value by using _.

```csharp
if (DateTime.TryParse("02/29/2019", out _))  
    Console.WriteLine("Date is valid");  
else  
    Console.WriteLine("Date is not valid");   
```
This ignores the out variable, but allows us to make use of the boolean result:
https://www.c-sharpcorner.com/blogs/c-sharp-hidden-gems-sharp1-discards-variable

