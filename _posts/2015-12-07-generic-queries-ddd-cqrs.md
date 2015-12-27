---
layout: post
title: Using 'dynamic' keyword to write generic query in DDD and CQRS
date: 2015-12-07 21:47
categories: CQRS DDD dynamic
---

Article text coming soon!
```csharp
// this function will only retrieve the required columns from table
public IEnumerable<dynamic> GenericGet(Func<Employee, dynamic> selectClause)
        {
            var a = db.Employee.Where(x => x.IsInitial == true).Select(selectClause).AsNoTracking();
            return a;
        }
        
		
```

Calling the function


```csharp
// calling the function  
        Func<Employee, dynamic> t = x => new { x.Name, x.Email };
        var qryEmployee_Name_Email = GenericGet(t);
```