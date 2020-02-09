# C# Coding Standards

In short examples that do not include [using directives](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-directive), use namespace qualifications. If you know that a namespace is imported by default in a project, you do not have to fully qualify the names from that namespace. Qualified names can be broken after a dot (.) if they are too long for a single line, as shown in the following example.

```csharp
var currentPerformanceCounterCategory = new System.Diagnostics.
    PerformanceCounterCategory();
```

You do not have to change the names of objects that were created by using the Visual Studio designer tools to make them fit other guidelines.

## Naming Conventions

The Microsoft [naming](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines) and [capitalization](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/capitalization-conventions) guidelines should be followed.

### Capitalising acronyms

Remember to apply proper naming conventions to any acronyms present in project names.

**DO** capitalise both characters on two-character acronyms, except the first word of a camel-cased identifier.  Do not confuse acronyms with abbreviations.  Abbreviations of "identifier" and "database" should be capitalised as "Id" and "Db" respectively.

```csharp
System.IO
public void StartIO(Stream ioStream)
```

**DO** capitalise only the first character of acronyms, except the first word of a camel-cased identifier.

```csharp
System.Xml
public void ProcessHtmlTag(string htmlTag)
```

**DO NOT** capitalise any of the characters of any acronyms, whatever their length, at the beginning of a camel-cased identifier.

```csharp
var xmlDocument = new XmlDocument();
var ioStream = new Stream();
var htmlClient = new HtmlClient();
```

Remember to follow the [namespace guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-namespaces).

For most APIs the following pattern will be appropriate for naming:

`OrganisationName.ProductName.Feature.Api`

Example:

`Codidact.InstanceManagement.Deployment.Api`

## Layout Conventions
Good layout uses formatting to emphasize the structure of your code and to make the code easier to read. Microsoft examples and samples conform to the following conventions:

- Use the default Code Editor settings (smart indenting, four-character indents, tabs saved as spaces). For more information, see [Options, Text Editor, C#, Formatting](https://docs.microsoft.com/en-us/visualstudio/ide/reference/options-text-editor-csharp-formatting).
- Write only one statement per line.
- Write only one declaration per line.
- If continuation lines are not indented automatically, indent them one tab stop (four spaces).
- Add at least one blank line between method definitions and property definitions.
- Always include braces in if/else, try/catch/finally statements.
- Do not use parentheses to make clauses in an expression apparent.

For simple statements, using parentheses aren't necessary.

```csharp
// this is unnecessary
if ((val1 > val2) && (val1 > val3))
 
// this is preferred
if (val1 > val2 && val1 > val3)
```

For more complex statements, the need for parentheses becomes a red flag (or a code smell), that tells you that the code needs to be refactored.

```csharp
// check if the order can be placed (it needs to be over the minimum order value, or the member is a prime member, or they have a family member who is)
if ((totalBasket + deliveryCost) > minimumOrderValue || (member.IsPrime || member.Family.Any(f => f.IsPrime))
```

Complex if statements, like above, should be broken down into separate assignments, using appropriately expressive names.  This makes the intention of the if statement more obvious and readable, and also aids debugging.

```csharp
// use assignments with expressive names
bool isOverMinimumValue = (totalBasket + deliveryCost) > minimumOrderValue;
bool isPrimeMember = member.IsPrime || member.Family.Any(f => f.IsPrime);
 
// now the intention of the if statement becomes more obvious to the reader, note how additional parentheses are no longer necessary
if (isOverMinimumValue || isPrimeMember)
```

## Commenting Conventions
Place the comment on a separate line, not at the end of a line of code.

Insert one space between the comment delimiter (//) and the comment text, as shown in the following example.

```csharp
// the following declaration creates a query, it does not run
// the query
Do not create formatted blocks of asterisks around comments.

## Language Guidelines
### String Data Type
Use the + operator to concatenate short strings, as shown in the following code.

```csharp
string displayName = nameList[n].LastName + ", " + nameList[n].FirstName;
```

To append strings in loops, especially when you are working with large amounts of text, use a [StringBuilder](https://docs.microsoft.com/en-us/dotnet/api/system.text.stringbuilder) object.

```csharp
var phrase = "lalalalalalalalalalalalalalalalalalalalalalalalalalalalalala";
var manyPhrases = new StringBuilder();
 
for (var i = 0; i < 10000; i++)
{
    manyPhrases.Append(phrase);
}
 
Console.WriteLine("tra" + manyPhrases);
```

### Implicitly Typed Local Variables
Use [implicit typing](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/implicitly-typed-local-variables) for local variables when the type of the variable is obvious from the right side of the assignment, or when the precise type is not important.

```csharp
// when the type of a variable is clear from the context, use var
// in the declaration
var var1 = "This is clearly a string.";
var var2 = 27;
var var3 = Convert.ToInt32(Console.ReadLine());
```

Do not use var when the type is not apparent from the right side of the assignment.

```csharp
// when the type of a variable is not clear from the context, use an
// explicit type
int var4 = ExampleClass.ResultSoFar();
```

Do not rely on the variable name to specify the type of the variable. It might not be correct.

```cshap
// naming the following variable inputInt is misleading,
// it is a string
var inputInt = Console.ReadLine();
Console.WriteLine(inputInt);
```

Avoid the use of var in place of [dynamic](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/dynamic).

Use implicit typing to determine the type of the loop variable in [for](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/for) and [foreach](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/foreach-in) loops.

The following example uses implicit typing in a for statement.

```csharp
var syllable = "ha";
var laugh = "";
 
for (var i = 0; i < 10; i++)
{
    laugh += syllable;
    Console.WriteLine(laugh);
}
```

The following example uses implicit typing in a foreach statement.

```csharp
foreach (var ch in laugh)
{
    if (ch == 'h')
    {
        Console.Write("H");
    }
    else
    {
        Console.Write(ch);
    }
}
 
Console.WriteLine();
```

### Unsigned Data Type
In general, use int rather than unsigned types. The use of int is common throughout C#, and it is easier to interact with other libraries when you use int.

### Arrays
Use the concise syntax when you initialize arrays on the declaration line.

```csharp
// preferred syntax, note that you cannot use var here instead of string[]
string[] vowels1 = { "a", "e", "i", "o", "u" };
 
// if you use explicit instantiation, you can use var
var vowels2 = new string[] { "a", "e", "i", "o", "u" };
 
// if you specify an array size, you must initialize every element
var vowels3 = new string[5] { "a", "e", "i", "o", "u" };
```

### Delegates
Use the concise syntax to create instances of a delegate type.

```csharp
// first, in class Program, define the delegate type and a method that 
// has a matching signature
 
// define the type
public delegate void Del(string message);
 
// define a method that has a matching signature
public static void DelMethod(string str)
{
    Console.WriteLine("DelMethod argument: {0}", str);
}
```

```csharp
// in the Main method, create an instance of Del
 
// preferred: Create an instance of Del by using a method group
Del exampleDel2 = DelMethod;
 
// the following declaration uses the full syntax
Del exampleDel1 = new Del(DelMethod);
```

### try-catch and using Statements in Exception Handling
Use a [try-catch](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/try-catch) statement for most exception handling.

```csharp
static string GetValueFromArray(string[] array, int index)
{
    try
    {
        return array[index];
    }
    catch (System.IndexOutOfRangeException ex)
    {
        Console.WriteLine("Index is out of range: {0}", index);
        throw;
    }
}
```

Simplify your code by using the C# [using statement](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-statement). If you have a [try-finally](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/try-finally) statement in which the only code in the finally block is a call to the [Dispose](https://docs.microsoft.com/en-us/dotnet/api/system.idisposable.dispose) method, use a using statement instead.

```csharp
// this try-finally statement only calls Dispose in the finally block
Font font1 = new Font("Arial", 10.0f);
try
{
    byte charset = font1.GdiCharSet;
}
finally
{
    if (font1 != null)
    {
        ((IDisposable)font1).Dispose();
    }
}
 
// you can do the same thing with a using statement
using (Font font2 = new Font("Arial", 10.0f))
{
    byte charset = font2.GdiCharSet;
}
```

### Use short circuiting logic for boolean operators (&& and ||)
To avoid exceptions and increase performance by skipping unnecessary comparisons, use && instead of & and || instead of | when you perform comparisons, as shown in the following example.

```csharp
Console.Write("Enter a dividend: ");
var dividend = Convert.ToInt32(Console.ReadLine());
 
Console.Write("Enter a divisor: ");
var divisor = Convert.ToInt32(Console.ReadLine());
 
// if the divisor is 0, the second clause in the following condition
// causes a run-time error. The && operator short circuits when the
// first expression is false. That is, it does not evaluate the
// second expression. The & operator evaluates both, and causes
// a run-time error when divisor is 0.
if (divisor != 0 && (dividend / divisor > 0))
{
    Console.WriteLine("Quotient: {0}", dividend / divisor);
}
else
{
    Console.WriteLine("Attempted division by 0 ends up here.");
}
```

### New Operator
Use the concise form of object instantiation, with implicit typing, as shown in the following declaration.

```csharp
var instance1 = new ExampleClass();
```

Use object initializers to simplify object creation.

```csharp
// object initializer
var instance3 = new ExampleClass
{
    Name = "Desktop",
    ID = 37414,
    Location = "Redmond",
    Age = 2.3,
};
 
// default constructor and assignment statements
var instance4 = new ExampleClass();
instance4.Name = "Desktop";
instance4.ID = 37414;
instance4.Location = "Redmond";
instance4.Age = 2.3;
```

### Event Handling
If you are defining an event handler in a program that needs to update the GUI on a specific thread, do not use lambda expressions for event handlers.

🛈 *This is because lambda expressions are defined within an implicit closure object, instead of the dispatcher object that it was defined in.  This prevents the event provider from detecting that the event needs to be synchronised to the GUI thread.  Method groups do belong to the defining object, and by using them, the event provider can check if the event target is a dispatcher object, and synchronise the event appropriately.*

Instead of using a lambda like this

```csharp
public Form2()
{
    // you can use a lambda expression to define an event handler
    this.Click += (s, e) =>
    {
        // this will throw an error, because the delegate target was not the dispather object, instead
        // it was hidden by the auto generated enclosure class for the lambda
        message.Text = "Clicked";
    };
}
```

Use a method group instead

```csharp
public Form1()
{
    // the method group target is this dispatcher object (i.e. Form1)
    this.Click += new EventHandler(Form1_Click);
}
 
void Form1_Click(object sender, EventArgs e)
{
    // this will now work, because the event raiser knew to invoke the event on the dispatcher
    message.Text = "Clicked";
}
```

### Static Members
Call [static](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/static) members by using the class name: `ClassName.StaticMember`. This practice makes code more readable by making static access clear.

Do not qualify a static member defined in a base class with the name of a derived class. While that code compiles, the code readability is misleading, and the code may break in the future if you add a static member with the same name to the derived class.

⚠️ *Avoid using static methods, because they are not mockable, and cannot be used with dependency injection.*

#### LINQ Queries
Use meaningful names for query variables. The following example uses seattleCustomers for customers who are located in Seattle.

```csharp
var seattleCustomers = from cust in customers
                       where cust.City == "Seattle"
                       select cust.Name;
```

Use aliases to make sure that property names of anonymous types are correctly capitalized, using Pascal casing.

```csharp
var localDistributors = from customer in customers
                        join distributor in distributors on customer.City equals distributor.City
                        select new
                        {
                            Customer = customer,
                            Distributor = distributor,
                        };
```

Rename properties when the property names in the result would be ambiguous. For example, if your query returns a customer name and a distributor ID, instead of leaving them as Name and ID in the result, rename them to clarify that Name is the name of a customer, and ID is the ID of a distributor.

```csharp
var localDistributors2 = from cust in customers
                         join dist in distributors on cust.City equals dist.City
                         select new
                         {
                             CustomerName = cust.Name,
                             DistributorID = dist.ID,
                         };
```

Use implicit typing in the declaration of query variables and range variables.

```csharp
var seattleCustomers = from cust in customers
                       where cust.City == "Seattle"
                       select cust.Name;
```

Use multiple from clauses instead of a [join](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/join-clause) clause to access inner collections. For example, a collection of Student objects might each contain a collection of test scores. When the following query is executed, it returns each score that is over 90, along with the last name of the student who received the score.

```csharp
// use a compound from to access the inner sequence within each element
var scoreQuery = from student in students
                 from score in student.Scores
                 where score > 90
                 select new
                 {
                     Last = student.LastName,
                     Score = score,
                 };
```

Use let statments to simplify LINQ queries, improve performance by preventing multiple evaluations, and improve readability.

```csharp
var scoreQuery = from student in students
                 from score in student.Scores
                 let isPassingMark = score > 90
                 where isPassingMark
                 select new
                 {
                     Last = student.LastName,
                     Score = score,
                     ResultText = isPassingMark ? "Pass" : "Fail",
                 };
```

### Optional commas
Always include the optional comma in object and collection initialisers.  This improves code maintainability, allowing for abritary lines to be added and removed from an initialiser without causing compilation errors.

```csharp
var member = new Member
{
    Firstname = "John",
    Surname = "Doe",
};
 
var numbers = new int []
{
    1,
    2,
    3,
};
 
enum Colour
{
    Red,
    Green,
    Blue,
}
```
