---
layout: post
title: Using 'dynamic' keyword to write generic query in DDD and CQRS
date: 2015-12-07 21:47
categories: CQRS DDD dynamic
---
The other day I was discussing with a friend about how we could write generic queries for the Query part of CQRS.
The friend was sick of writing a query for each view because the application which he was working on had lots off different combinations. 
In fact one of the requirements in his application meant that the user could create his own query from the UI, we discussed a lot of ways we could write a generic query function and if we could use the new 'dynamic' keyword feature from the language.

This is what we came up with.

```csharp
// this function will only retrieve the required columns from table
public IEnumerable<dynamic> GenericGet(
								Func<Employee, dynamic> selectClause)
        {
            var a = db.Employee
						.Where(x => x.IsInitial == true)
						.Select(selectClause).AsNoTracking();
            return a;
        }
```
The code above will make sure that only the columns requested in the select clause are requested from the SQL server.
It also makes sure that you dont have to write a class for return type of each query instead using dynamic.
Having said that we still wanted to make sure that we get the column names right and that the compiler catches any issues on it.

The calling functions shown below helps with this.


```csharp
// calling the function  
        Func<Employee, dynamic> t = emp => new { emp.Name, emp.Email };
        var qryEmployee_Name_Email = GenericGet(t);
```

Now its obvious the disadvantages this code has , the most annoying one being it has no compile time safety , if the code tries to access a property in the result and it  does not exist , it will throw a runtime exception instead of a compile time error.

We ended up discussing the requirement and coming up with a simpler UI to solve the issue but hey if this rocks your boat it rocks your boat.
s