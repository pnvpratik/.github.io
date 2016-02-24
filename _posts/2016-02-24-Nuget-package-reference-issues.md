
---
layout:     post
title:      Reference issues with Nuget packages
date:       2016-02-24
summary:    Reference issues with Nuget packages
categories: Nuget References 
---


We have a big solution file with around 80 projects, recently we seem to keep running into issues where a update on a nuget package by one developer will lead to reference issues for another developer.
The typical sign of this is Warning symbol ( yellow triangle with exclamation mark) next to a Reference in the Project > Reference section when you would expect the reference to be present.


The best way to resolve these issues is to force Nuget to reinstall all the Nuget packages on the machine.

Run the following command on the Package manager console to fix this issue.

```shell
Update-Package -ProjectName 'ProjectNameGoesHere' -Reinstall

Update-Package -Reinstall

```