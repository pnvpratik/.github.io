---
layout: post
title: Using 'dynamic' keyword to write generic query in DDD and CQRS
date: 2015-12-07 21:47
categories: CQRS DDD dynamic
---

// this function will only retrieve the required columns from table
public IEnumerable<dynamic> GenericGet(Func<Employee, dynamic> selectClause)
        {
            var a = db.WorkflowTask.Where(x => x.IsInitial == true).Select(selectClause).AsNoTracking();
            return a;
        }
        // calling the function  
        Func<Employee, dynamic> t = x => new { x.Name, x.Email };
        var qryEmployee_Name_Email = GenericGet(t);