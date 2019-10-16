Engineering guidelines
======================

* [Basics](#basics)
* [Source code management](#source-code-management)
* [Coding guidelines](#coding-guidelines)
* [Common Patterns](#common-patterns)
* [Testing PRs](#testing-prs)
* [Product planning and issue tracking](#product-planning-and-issue-tracking)
* [Breaking changes](#breaking-changes)

## Basics

### Copyright header and license notice

All source code files require this exact header (please do not make any changes to it):

```c#
// Copyright (c) DFDS A/S. All rights reserved.
// Licensed under the Apache License, Version 2.0. See License.txt in the project root for license information.
```

It is ok to skip it on generated files, such as `*.designer.cs`.

Every repo also needs the Apache 2.0 License in a file called LICENSE.txt in the root of the repo. Please use only identical copies to what we have in other repos.

(There are some exceptions to this, but they need to be approved by @cag)


### External dependencies

This refers to dependencies on projects (i.e. NuGet packages) outside of the current repo, and especially outside of DFDS.

Dependencies are managed in `build\external-dependencies.props`.

*Adding* or *updating* any external dependency requires consensus amongst your team members. After you have achieved consensus you must make sure the `build\external-dependencies.props` file in your repository reflects the dependency you're adding.

Dependencies that are used only in test projects and build tools are not nearly as rigid.


### Code reviews and checkins

To help ensure that only the highest quality code makes its way into the project, please submit all your code changes to GitHub as PRs. This includes runtime code changes, unit test updates, and updates to official samples. For example, sending a PR for just an update to a unit test might seem like a waste of time but the unit tests are just as important as the product code and as such, reviewing changes to them is also just as important. This also helps create visibility for your changes so that others can observe what is going on.

The advantages are numerous: improving code quality, more visibility on changes and their potential impact, avoiding duplication of effort, and creating general awareness of progress being made in various areas.

In general a PR should be signed off (using GitHub's "approve" feature) by the Subject Matter Expert (SME) of that code. For example, a change to the Banana project should be signed off by `@MrMonkey`, and not by `@MrsGiraffe`. If you don't know the SME, please talk to one of the engineering leads and they will be happy to help you identify the SME. Of course, sometimes it's the SME who is making a change, in which case a secondary person will have to sign off on the change (e.g. `@JuniorMonkey`).

To commit the PR to the repo either use GitHub's "Squash and Merge" button on the main PR page, or do a typical push that you would use with Git (e.g. local pull, rebase, merge, push).


## Source code management

:grey_exclamation: The *structure* of the code that we write and the *tools* that we use to write the code.


### Repos

To create a new repo in the https://github.com/dfds/ org, contact Development Excellence(TODO: SLACK LINK).


### Branch strategy

In general:

* `master` has the code that is being worked on but not yet released. This is the branch into which developers normally submit pull requests and merge changes into.

* `release/<major>.<minor>` contains code that is intended for release. The `<major>.<minor>` part of the branch name indicates the beginning of the version of the product the code will end up in. The branch may be used for several patch releases with the same major.minor number. It is common for repos to have multiple `release/*` branches, each which may receive servicing updates.


#### Making changes to release branches

If you make a change to a `release/*` branch directly, you typically also need to merge those changes back to `master`. This often causes some merge conflicts, so make sure to proceed carefully. If you're not sure how to do this, follow these steps.


##### Manual merges to master

```sh
# Make your changes to release/x.y
git checkout release/x.y    
git merge --ff-only your-name/my-approved-PR-branch
git push

# Make sure master is up to date first
git checkout master
git pull 

# Prepare your merge on a separate branch

git checkout --branch your-name/merge-release-x-y-to-master
git merge --no-commit release/x.y

# This is where you need to manually check your changes are good

git commit -m "Merge branch release/x.y into master"
git push -u origin your-name/merge-release-x-y-to-master

# Open a PR to review the changes

# Once approved
git checkout master
git merge --ff-only your-name/merge-release-x-y-to-master
git push
```

### Solution and project folder structure and naming

Solution files go in the repo root.

Solution names match repo names (e.g. Mvc.sln in the Mvc repo).

Solutions need to contain solution folders that match the physical folders (`src`, `test`, etc.).

For example, in the `Fruit` repo with the `Banana` and `Lychee` projects you would have these files checked in:

```
/Fruit.sln
/src/DFDS.Banana/Banana.csproj
/src/DFDS.Banana/Banana.cs
/src/DFDS.Banana/Util/BananaUtil.cs
/src/DFDS.Lychee/Lychee.csproj
/src/DFDS.Lychee/Lychee.cs
/src/DFDS.Lychee/Util/LycheeUtil.cs
/test/DFDS.Banana.Tests/BananaTest.csproj
/test/DFDS.Banana.Tests/BananaTest.cs
/test/DFDS.Banana.Tests/Util/BananaUtilTest.cs
```


### Conditional compilation for multiple Target Frameworks

Code sometimes has to be written to be target framework-specific due to API changes between frameworks. Use `#if` statements to create these conditional code segments:
Desktop:

```c#
#if NET46
            Console.WriteLine("Hi .NET 4.6");
#elif NETCOREAPP2_0
            Console.WriteLine("Hi .NET Standard 2.0");
#else
#error Target framework needs to be updated
#endif
```

Note the `#error` section that is present in case the target frameworks change - this ensure that we don't have dead code in the projects, and also no missing conditions.


### Assembly naming pattern

The general naming pattern is `dfds.<capability>.<area>.<subarea>`.


### Unit tests

We use xUnit.net for all unit testing.


### Repo-specific Samples

Some repos will have their own sample projects that are used for testing purposes and experimentation. Please ensure that these go in a `samples/` sub-folder in the repo.


## Coding guidelines

:grey_exclamation: The *content* of the code that we write.


### Coding style guidelines – general

The most general guideline is that we use all the VS default settings in terms of code formatting, except that we put `System` namespaces before other namespaces (this used to be the default in VS, but it changed in a more recent version of VS).

1. Use four spaces of indentation (no tabs)
2. Use `_camelCase` for private fields
3. Avoid `this.` unless absolutely necessary
4. Always specify member visibility, even if it's the default (i.e. `private string _foo;` not `string _foo;`)
5. Open-braces (`{`) go on a new line
6. Always use a curly brace scope in an if statement, even if it conditions a single statement.
7. Use any language features available to you (expression-bodied members, throw expressions, tuples, etc.) as long as they make for readable, manageable code.
   * This is pretty bad: `public (int, string) GetData(string filter) => (Data.Status, Data.GetWithFilter(filter ?? throw new ArgumentNullException(nameof(filter))));`
8. Avoid putting multiple classes in a single file. Use partial classes instead.
9. Avoid files that are longer then 500 lines (excluding machine generated code)
10. Avoid methods with more then 200 lines of code.
11. Lines should not exceed 200 chars.
12. Do not manually edit machine generated code. If you have to, use partial classes to cordon of the maintained portions.
13. With the exception of 0 and 1, never hard-code a numeric value; always declare a constant instead.
14. Whenever possible place string resources in .resx files.

Furthermore we encourage the use of .editorconfig and maintain a DFDS specific configuration file [here](TODO).
   

### Usage of the var keyword

The `var` keyword is to be used as much as the compiler will allow. For example, these are correct:

```c#
var fruit = "Lychee";
var fruits = new List<Fruit>();
var flavor = fruit.GetFlavor();
string fruit = null; // can't use "var" because the type isn't known (though you could do (string)null, don't!)
const string expectedName = "name"; // can't use "var" with const
```

The following are incorrect:

```c#
string fruit = "Lychee";
List<Fruit> fruits = new List<Fruit>();
FruitFlavor flavor = fruit.GetFlavor();
```


### Use C# type keywords in favor of .NET type names

When using a type that has a C# keyword the keyword is used in favor of the .NET type name. For example, these are correct:

```c#
public string TrimString(string s) {
    return string.IsNullOrEmpty(s)
        ? null
        : s.Trim();
}

var intTypeName = nameof(Int32); // can't use C# type keywords with nameof
```

The following are incorrect:

```c#
public String TrimString(String s) {
    return String.IsNullOrEmpty(s)
        ? null
        : s.Trim();
}
```


### Using default parameters 

When specifying default parameter values always prioritize using the [default literal](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/default) over value assignments and when opting for value assignment always use natural immutable constants (0, false, null). For example:

```c#
public string TrimString(string s = default) {
    return string.IsNullOrEmpty(s)
        ? null
        : s.Trim();
}
```

The following value assignment is less desired then the above:

```c#
public String TrimString(string s = "") {
    return String.IsNullOrEmpty(s)
        ? null
        : s.Trim();
}
```


### Exception handling

Catch only exceptions for which you have explicit exception handling and make sure that catch statements always rethrow the original exception (or an exception constructed using the original exception) to maintain the stack location of the original error. For example:

```c#
try
{
	DoWork();
}
catch(IOException ie)
{
	throw;
}
```


### Avoid function calls in boolean conditional statements. Assign into local variable and check them.

```c#
bool IsEverythingOk()
{...}
//Avoid:
if(IsEverythingOk())
{...}
//Correct:
var ok = IsEverythingOk();

if(ok)
{...}
```


### Avoid explicit casting. Use the `as` operator to defensively cast to a type. For example:

```c#
Animal animal = new Dog();
var dog = animal as Dog;

if(dog != null)
{...}

```


### Cross-platform coding

Our frameworks should work on CoreCLR, which supports multiple operating systems. Don't assume we only run (and develop) on Windows. Code should be sensitive to the differences between OS's. Here are some specifics to consider.

#### Line breaks
Windows uses `\r\n`, OS X and Linux uses `\n`. When it is important, use `Environment.NewLine` instead of hard-coding the line break.

Note: this may not always be possible or necessary. 

Be aware that these line-endings may cause problems in code when using `@""` text blocks with line breaks.

#### Environment Variables

OS's use different variable names to represent similar settings. Code should consider these differences.

For example, when looking for the user's home directory, on Windows the variable is `USERPROFILE` but on most Linux systems it is `HOME`.

```c#
var homeDir = Environment.GetEnvironmentVariable("USERPROFILE") 
                  ?? Environment.GetEnvironmentVariable("HOME");
```

Keep in mind that wherever possible environment variables should be accessed via the configuration system in .NET Core () by enabling the appropriate [configuration provider](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-3.0#environment-variables-configuration-provider).

#### File path separators

Windows uses `\` and OS X and Linux use `/` to separate directories. Instead of hard-coding either type of slash, use [`Path.Combine()`](https://msdn.microsoft.com/en-us/library/system.io.path.combine(v=vs.110).aspx) or [`Path.DirectorySeparatorChar`](https://msdn.microsoft.com/en-us/library/system.io.path.directoryseparatorchar(v=vs.110).aspx).

If this is not possible (such as in scripting), use a forward slash. Windows is more forgiving than Linux in this regard.


### When to use internals vs. public and when to use InternalsVisibleTo

As a modern set of frameworks, usage of internal types and members is allowed, but discouraged.

`InternalsVisibleTo` is used only to allow a unit test to test internal types and members of its runtime assembly. We do not use `InternalsVisibleTo` between two runtime assemblies.

If two runtime assemblies need to share common helpers then we will use a "shared source" solution with build-time only packages. 

If two runtime assemblies need to call each other's APIs, the APIs must be public. If we need it, it is likely that our customers need it.


### Async method patterns

By default all async methods must have the `Async` suffix. There are some exceptional circumstances where a method name from a previous framework will be grandfathered in.

Passing cancellation tokens is done with an optional parameter with a value of `default(CancellationToken)`, which is equivalent to `CancellationToken.None`. The main exception to this is in web scenarios where there is already an `HttpContext` being passed around, in which case the context has its own cancellation token that can be used when needed.

Sample async method:

```c#
public Task GetDataAsync(
    QueryParams query,
    int maxData,
    CancellationToken cancellationToken = default(CancellationToken))
{
    ...
}
```


### Extension method patterns

The general rule is: if a regular static method would suffice, avoid extension methods.

Extension methods are often useful to create chainable method calls, for example, when constructing complex objects, or creating queries.

Internal extension methods are allowed, but bear in mind the previous guideline: ask yourself if an extension method is truly the most appropriate pattern.

The namespace of the extension method class should generally be the namespace that represents the functionality of the extension method, as opposed to the namespace of the target type.

The class name of an extension method container (also known as a "sponsor type") should generally follow the pattern of `<Feature>Extensions`, `<Target><Feature>Extensions`, or `<Feature><Target>Extensions`. For example:

```c#
namespace Food {
    class Fruit { ... }
}

namespace Fruit.Eating {
    class FruitExtensions { public static void Eat(this Fruit fruit); }
  OR
    class FruitEatingExtensions { public static void Eat(this Fruit fruit); }
  OR
    class EatingFruitExtensions { public static void Eat(this Fruit fruit); }
}
```

When writing extension methods for an interface the sponsor type name must not start with an `I`.


### Minimize code in application assemblies (EXE client assemblies) and use .NET Standard libraries instead to contain business logic.

In general we want to avoid bundling our business logic with the application assemblies as it becomes impossible to re-use business logic in other host contexts without inheriting undesired dependencies. 


### Doc comments

The person writing the code will write the doc comments. Public APIs only. No need for doc comments on non-public types.

Note: Public means callable by a customer, so it includes protected APIs. However, some public APIs might still be "for internal use only" but need to be public for technical reasons. We will still have doc comments for these APIs but they will be documented as appropriate.


### Unit tests and functional tests


#### Assembly naming

The unit tests for the `Dfds.Fruit` assembly live in the `Dfds.Fruit.Tests` assembly.

The integration tests for the `Dfds.Fruit` assembly live in the `Dfds.Fruit.IntegrationTests` assembly.

The functional tests (end-2-end) for the `Dfds.Fruit` assembly live in the `Dfds.Fruit.FunctionalTests` assembly.

In general there should be exactly one unit test assembly for each product runtime assembly. In general there should be one integration/functional test assembly per repo. Exceptions can be made for both.


#### Unit test class naming

Test class names end with `Test` and live in the same namespace as the class being tested. For example, the unit tests for the `Dfds.Fruit.Banana` class would be in a `Dfds.Fruit.BananaTest` class in the test assembly.


#### Unit test method naming

Unit test method names must be descriptive about *what is being tested*, *under what conditions*, and *what the expectations are*. Pascal casing and underscores can be used to improve readability. The following test names are correct:

```
PublicApiArgumentsShouldHaveNotNullAnnotation
Public_api_arguments_should_have_not_null_annotation
```

The following test names are incorrect:

```
Test1
Constructor
FormatString
GetData
```


#### Unit test structure

The contents of every unit test should be split into three distinct stages, optionally separated by these comments:

```c#
// Arrange  
// Act  
// Assert 
```

The crucial thing here is that the `Act` stage is exactly one statement. That one statement is nothing more than a call to the one method that you are trying to test. Keeping that one statement as simple as possible is also very important. For example, this is not ideal:

```c#
int result = myObj.CallSomeMethod(GetComplexParam1(), GetComplexParam2(), GetComplexParam3());
```

This style is not recommended because way too many things can go wrong in this one statement. All the `GetComplexParamN()` calls can throw for a variety of reasons unrelated to the test itself. It is thus unclear to someone running into a problem why the failure occurred.

The ideal pattern is to move the complex parameter building into the `Arrange` section:

```c#
// Arrange
P1 p1 = GetComplexParam1();
P2 p2 = GetComplexParam2();
P3 p3 = GetComplexParam3();

// Act
int result = myObj.CallSomeMethod(p1, p2, p3);

// Assert
Assert.AreEqual(1234, result);
```

Now the only reason the line with `CallSomeMethod()` can fail is if the method itself blew up. This is especially important when you're using helpers such as `ExceptionHelper`, where the delegate you pass into it must fail for exactly one reason.


### Testing exception messages

In general testing the specific exception message in a unit test is important. This ensures that the exact desired exception is what is being tested rather than a different exception of the same type. In order to verify the exact exception it is important to verify the message.

To make writing unit tests easier it is recommended to compare the error message to the RESX resource. However, comparing against a string literal is also permitted.

```c#
var ex = Assert.Throws<InvalidOperationException>(
    () => fruitBasket.GetBananaById(1234));
Assert.Equal(
    Strings.FormatInvalidBananaID(1234),
    ex.Message);
```


#### Use xUnit.net's plethora of built-in assertions

xUnit.net includes many kinds of assertions – please use the most appropriate one for your test. This will make the tests a lot more readable and also allow the test runner report the best possible errors (whether it's local or the CI machine). For example, these are bad:

```c#
Assert.Equal(true, someBool);

Assert.True("abc123" == someString);

Assert.True(list1.Length == list2.Length);

for (int i = 0; i < list1.Length; i++) {
    Assert.True(
        String.Equals
            list1[i],
            list2[i],
            StringComparison.OrdinalIgnoreCase));
}
```

These are good:

```c#
Assert.True(someBool);

Assert.Equal("abc123", someString);

// built-in collection assertions!
Assert.Equal(list1, list2, StringComparer.OrdinalIgnoreCase);
```


#### Parallel tests

By default all unit test assemblies should run in parallel mode, which is the default. Unit tests shouldn't depend on any shared state, and so should generally be runnable in parallel. If the tests fail in parallel, the first thing to do is to figure out *why*; do not just disable parallel tests!

For functional tests it is reasonable to disable parallel tests.


### Use only complete words or common/standard abbreviations in public APIs

Public namespaces, type names, member names, and parameter names must use complete words or common/standard abbreviations.

These are correct:
```c#
public void AddReference(AssemblyReference reference);
public EcmaScriptObject SomeObject { get; }
```

These are incorrect:
```c#
public void AddRef(AssemblyReference ref);
public EcmaScriptObject SomeObj { get; }
```

## Common Patterns

This section contains common patterns used in our code.

### Logging patterns

1. Always specify an `EventId`. Include a numeric ID **and** a name. The name should be a `PascalCasedCompoundWord` (i.e. no spaces, and each "word" within the name starts with a capital letter).

1. In production code, use "pre-compiled logging functions" (see below). Test code can use any kind of logging

1. Prefer defining pre-compiled messages in a static class named `Log` that is a nested class within the class you are logging from. Messages that are used by multiple components can be defined in a shared class (but this is discouraged).

1. Consider separating the `Log` nested class into a separate file by using `partial` classes. Name the additional file `[OriginalClassName].Log.cs`

1. Never use string-interpolation (`$"Foo {bar}"`) for log messages. Log message templates are designed so that structured logging systems can make individual parameters queryable and string-interpolation breaks this.

1. Always use `PascalCasedCompoundWords` as template replacement tokens.

**Pre-compiled Logging Functions**

Production code should use "pre-compiled" logging functions. This means using `LoggerMessage.Define` to create a compiled function that can be used to perform logging. For example, consider the following log statement:

```csharp
public class MyComponent
{
	public void MyMethod()
	{
		_logger.LogError(someException, new EventId(1, "ABadProblem"), "You are having a bad problem and will not go to {Location} today", "Space");
	}
}
```

The logging infrastructure has to parse the template (`"You are having a bad problem and will not go to {Location} today"`) every time the log is written. A pre-compiled logging function allows you to compile the template once and get back a delegate that can be invoked to log the message without requiring the template be re-parsed. For example:

```csharp
public class MyComponent
{
	public void MyMethod()
	{
		Log.ABadProblem(_logger, "Space", someException);
	}

	private static class Log
	{
		private static readonly Action<ILogger, string, Exception> _aBadProblem =
			LoggerMessage.Define<string>(
				LogLevel.Error,
				new EventId(2, "ABadProblem"), 
				"You are having a bad problem and will not go to {Location} today");

		public static void ABadProblem(ILogger logger, string location, Exception ex) => _aBadProblem(logger, location, ex);
	}
}
```

If `MyComponent` is a large class, consider splitting the `Log` nested class into a separate file:

**MyComponent.cs**

```csharp
public partial class MyComponent
{
	public void MyMethod()
	{
		Log.ABadProblem(_logger, "Space", someException);
	}
}
```

**MyComponent.Log.cs**

```csharp
public partial class MyComponent
{
	private static class Log
	{
		private static readonly Action<ILogger, string, Exception> _aBadProblem =
			LoggerMessage.Define<string>(
				LogLevel.Error,
				new EventId(2, "ABadProblem"), 
				"You are having a bad problem and will not go to {Location} today");

		public static void ABadProblem(ILogger logger, string location, Exception ex) => _aBadProblem(logger, location, ex);
	}
}
```

## Product planning and issue tracking

:grey_exclamation: How we track what work there is to do.


### Issue tracking

Bug management takes place in GitHub. Each repo has its own issue tracker. Bugs cannot be moved between repos so make sure you open a bug in the right repo. To "port" a bug to another repo, consider using a tool such as https://github-issue-mover.appspot.com/.


### Breaking changes

In general, breaking changes can be made only in a new **major** product version, e.g. moving from `1.x.x` to `2.0.0`. Even still, we generally try to avoid breaking changes because they can incur large costs for anyone using these products.

Breaking changes in major versions need to be approved by an engineering manager.

If there is a case where a breaking change needs to be made *not* in a major product update, the change must be approved by an engineering manager and an enterprise architect.

For the normal case of breaking changes in major versions, this is the ideal process:

1. Provide some new alternative API (if necessary)
2. Mark the old type/member as `[Obsolete]` to alert users (see below), and to point them at the new alternative API (if applicable)
 * If the old API really doesn't/can't work at all, please discuss with engineering team
3. Update the XML doc comments to indicate the type/member is obsolete, plus what the alternative is. This is typically exactly the same as the obsolete attribute message.
4. File a bug in the next major milestone (e.g. 2.0.0) to remove the type/member
 * Mark this bug with a red `[breaking-change]` label (use exact casing, hyphenation, etc.). Create the label in the repo if it's not there.

Example of obsoleted API:

```c#
    /// <summary>
    /// <para>
    ///     This method/property/type is obsolete and will be removed in a future version.
    ///     The recommended alternative is Microsoft.SomethingCore.SomeType.SomeNewMethod.
    /// </para>
    /// <para>
    ///     ... old docs...
    /// </para>
    /// </summary>
    [Obsolete("This method/property/type is obsolete and will be removed in a future version. The recommended alternative is Microsoft.SomethingCore.SomeType.SomeNewMethod.")]
    public void SomeOldMethod(...)
    {
        ...
    }
```
