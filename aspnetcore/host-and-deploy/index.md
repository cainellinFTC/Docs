---
title: Host and deploy ASP.NET Core
author: guardrex
description: Learn how to set up hosting environments and deploy ASP.NET Core apps.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: host-and-deploy/index
---
# Host and deploy ASP.NET Core

In general, to deploy an ASP.NET Core app to a hosting environment:

* Deploy the published app to a folder on the hosting server.
* Set up a process manager that starts the app when requests arrive and restarts the app after it crashes or the server reboots.
* For configuration of a reverse proxy, set up a reverse proxy to forward requests to the app.

## Publish to a folder

The [dotnet publish](/dotnet/core/tools/dotnet-publish) command compiles app code and copies the files required to run the app into a *publish* folder. When deploying from Visual Studio, the `dotnet publish` step occurs automatically before the files are copied to the deployment destination.

### Folder contents

The *publish* folder contains one or more app assembly files, dependencies, and optionally the .NET runtime.

A .NET Core app can be published as *self-contained deployment* or *framework-dependent deployment*. If the app is self-contained, the assembly files that contain the .NET runtime are included in the *publish* folder. If the app is framework-dependent, the .NET runtime files aren't included because the app has a reference to a version of .NET that's installed on the server. The default deployment model is framework-dependent. For more information, see [.NET Core application deployment](/dotnet/core/deploying/).

In addition to *.exe* and *.dll* files, the *publish* folder for an ASP.NET Core app typically contains configuration files, static assets, and MVC views. For more information, see <xref:host-and-deploy/directory-structure>.

## Set up a process manager

An ASP.NET Core app is a console app that must be started when a server boots and restarted if it crashes. To automate starts and restarts, a process manager is required. The most common process managers for ASP.NET Core are:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows Service](xref:host-and-deploy/windows-service)

## Set up a reverse proxy

::: moniker range=">= aspnetcore-2.0"

If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) server, [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) can be used as a reverse proxy server. A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.

Either configuration&mdash;with or without a reverse proxy server&mdash;is a supported hosting configuration for ASP.NET Core 2.0 or later apps. For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) server and will be exposed to the Internet, use [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) as a reverse proxy server. A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel. The main reason for using a reverse proxy is security. For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

## Proxy server and load balancer scenarios

Additional configuration might be required for apps hosted behind proxy servers and load balancers. Without additional configuration, an app might not have access to the scheme (HTTP/HTTPS) and the remote IP address where a request originated. For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).

## Use Visual Studio and MSBuild to automate deployments

Deployment often requires additional tasks besides copying the output from [dotnet publish](/dotnet/core/tools/dotnet-publish) to a server. For example, extra files might be required or excluded from the *publish* folder. Visual Studio uses MSBuild for web deployment, and MSBuild can be customized to do many other tasks during deployment. For more information, see <xref:host-and-deploy/visual-studio-publish-profiles> and the [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) book.

By using [the Publish Web feature](xref:tutorials/publish-to-azure-webapp-using-vs) or [built-in Git support](xref:host-and-deploy/azure-apps/azure-continuous-deployment), apps can be deployed directly from Visual Studio to the Azure App Service. Azure DevOps Services supports [continuous deployment to Azure App Service](/azure/devops/pipelines/targets/webapp). For more information, see [DevOps with ASP.NET Core and Azure](xref:azure/devops/index).

## Publish to Azure

See <xref:tutorials/publish-to-azure-webapp-using-vs> for instructions on how to publish an app to Azure using Visual Studio. The app can also be published to Azure from the [command line](/azure/app-service/app-service-web-get-started-dotnet).

## Host in a web farm

For information on configuration for hosting ASP.NET Core apps in a web farm environment (for example, deployment of multiple instances of your app for scalability), see <xref:host-and-deploy/web-farm>.

::: moniker range=">= aspnetcore-2.2"

## Perform health checks

Use Health Check Middleware to perform health checks on an app and its dependencies. For more information, see <xref:host-and-deploy/health-checks>.

::: moniker-end

## Additional resources

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
