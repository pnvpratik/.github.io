---
layout:     post
title:      How to use localization RESX in MVC 6
date:       2015-12-25
summary:    Localization in MVC6.
categories: MVC6 Localization 
---

A few months  ago I asked  a  question on stack overflow about how  to implement Localization in MVC 6 , unfortunately at that time MVC 6 was still in Beta 7 and the Localizaton services were not complete.
Now that localization  has been implemented by Microsoft guys , its  worth doing a quick blog on how to implement Localization in MVC 6.


## Step 1

The first step is to include the dependency for Localization Nuget package in project.json

```json
  "Microsoft.AspNet.Localization": "1.0.0-*"
```
## Step 2

MVC 6 has taken IOC to the next level. To an extent that everything has to be included in the baked in Dependency injector.

The way to include Localization is to  use  the `AddViewLocalization()` and `AddDataAnnotationsLocalization()` functions

```csharp
       services.AddLocalization(options => 
	   				options.ResourcesPath = "Resources");
       services.AddMvc()
               .AddViewLocalization()
               .AddDataAnnotationsLocalization();
```

## Step 3 

We must now configure the Localization services we have added. 

```csharp

var requestLocalizationOptions = new RequestLocalizationOptions
            {
                SupportedCultures = new List<CultureInfo>
                {
                    new CultureInfo("hi-IN"),
					new CultureInfo("en-GB"),
                },
                SupportedUICultures = new List<CultureInfo>
                {
                    new CultureInfo("hi-IN"),
					new CultureInfo("en-GB"),
                }
            };

app.UseRequestLocalization(requestLocalizationOptions,
				 new RequestCulture(new CultureInfo("hi-IN")));
```

The code above first sets 2 supported cultures. The `UseRequestLocalization` function  sets the  default  culture  and the options well.



## Put it all to use 

Now  that we are done  with  all the  configuration , we need to put it all to use.

```csharp

 public class LoginViewModel
    {
        [Required(ErrorMessageResourceName ="EmailRequired",
			ErrorMessageResourceType 
				= typeof(MVC6_RESX.Resources.Resource))]
        public string Email { get; set; }
		...
	}
```

Note : This  code is  working on 25 Dec 2015 with MVC 6 RC1. 