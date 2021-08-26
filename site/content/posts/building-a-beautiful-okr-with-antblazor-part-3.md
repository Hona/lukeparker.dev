+++
authors = ["Luke Parker"]
categories = ["Development"]
date = 2021-08-25T14:00:00Z
series = ["OKR"]
slug = "building-a-beautiful-okr-with-antblazor/2"
tags = ["OKR", "C#", "AntBlazor", "AntDesign", "Blazor"]
title = "Building a Beautiful OKR with AntBlazor - Part 3"

+++
## Setup AntBlazor

Before we can get started using the many great components provided by [AntBlazor](https://antblazor.com), we have to first install it and follow the [setup steps](https://antblazor.com/en-US/docs/introduce).

### NuGet

Using your package manager, add a reference to the latest version of `AntDesign` in the project `OKR.Web`

### Blazor Boilerplate

Now with the package installed, we have to reference the assets and add a few modules to the dependency injection.

Add to OKR.Web/Pages/_Host.cshtml, just before the <head> tag closes:

```html
<link href="_content/AntDesign/css/ant-design-blazor.css" rel="stylesheet" />
<script src="_content/AntDesign/js/ant-design-blazor.js"></script>
```

This references the default CSS file - you can replace it to easily get [dark mode](https://ant.design/docs/react/customize-theme), or a custom theme. There is also some JS interop required hence the import.

OKR.Web/Startup.cs
  
```cs
services.AddAntDesign();
```
  
This is an extension method provided in the library, which adds the services all in one clean call. Some of the services added here is the message service, modal service, etc.

OKR.Web/App.razor

```html
<AntContainer />
```

At the bottom of the file add this line, this allows the popups to run globally and persist on page changes, as well as [other uses](https://github.com/ant-design-blazor/ant-design-blazor/blob/master/components/core/AntContainer.razor).

Finally, we need to add the AntBlazor namespace to the Blazor component imports:

OKR.Web/_Imports.razor

```cs
@using AntDesign
```

That's it! We are now ready to build with these enterprise grade components.

### Removing Bootstrap

A small caveat of the Blazor template provided by .NET is that all projects begin with Bootstrap and Open Iconic. The CSS file conflicts with AntBlazor and causes some centring issues. 

As we aren't using Bootstrap or Open Iconic in this project at all, we can just delete both of them completely:

* OKR.Web/wwwroot/css/bootstrap
* OKR.Web/wwwroot/css/open-iconic

### Removing unused CSS

Now that there is no bootstrap, there was some custom CSS that we don't use, so lets clean it up.

OKR.Web/wwwroot/css/site.css

```css
  #blazor-error-ui {
      background: lightyellow;
      bottom: 0;
      box-shadow: 0 -1px 2px rgba(0, 0, 0, 0.2);
      display: none;
      left: 0;
      padding: 0.6rem 1.25rem 0.7rem 1.25rem;
      position: fixed;
      width: 100%;
      z-index: 1000;
  }

      #blazor-error-ui .dismiss {
          cursor: pointer;
          position: absolute;
          right: 0.75rem;
          top: 0.5rem;
      }
```

Delete everything from the file leaving just the Blazor error UI - which is the bar on the bottom. 

![](/uploads/blazor-error-ui.PNG)

## Using Forwarded Headers

As we are proxying from HTTPS/domain to the project using NGINX, the application needs to change the pipeline so that redirects are correct, and HTTPS is correctly detected. We also don't need to force redirects to HTTPS as it is already forced using NGINX.

Just before `app.UseStaticFiles();` add the following:

OKR.Web/Startup.cs

```cs
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders =
        ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});
```

Then, delete the redirection: `app.UseHttpsRedirection();`

## Verify AntBlazor is Working

Inside of Index.razor, there is currently a simple 'Hello World', lets test everything is working correctly by displaying a simple button:

OKR.Web/Pages/Index.razor

```html
<Button Type="primary">Primary</Button>
```

You should see the following:

![](/uploads/antblazor-primary-button.PNG)

If you got this far, you are doing great!

***

In the next post, I continue the UI layout with AntBlazor.

[← Part 2](https://lukeparker.dev/posts/building-a-beautiful-okr-with-antblazor/2) | [Part 4 →](https://lukeparker.dev/posts/building-a-beautiful-okr-with-antblazor/4)