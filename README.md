MiniProfiler.AzureCacheStorage
==============================
## Background ##

When using [MiniProfiler](https://github.com/SamSaffron/MiniProfiler) in an application hosted on multiple instances, MiniProfiler receives a 404 when POST is done to /mini-profiler-resources/results.

The reason is because the default implementation of the IStorage provider uses HttpRuntime.Cache. This cache is machine specific, and as a result instance B **cannot** access the MiniProfiler data saved on instance A. 

This is an implementation of MiniProfiler.IStorage that uses the Azure distributed cache for temporarily saving results.


### Assumtions ###
Using this couldn't be simpler, but does assume a few of  things  

1. You have already have an Azure Cahe subscription
2. You have the Azure Cache assemblies referenced in your project. The easiest way to do this is using the [official nuget package](http://nuget.org/packages/WindowsAzure.Caching)
3. You have the *dataCacheClient* configuration set in your config. The nuget package above will add this for you. You will just need to add your specific information to it.

### Setup ###

1. Copy the `AzureCacheStorage` file in the repo to your project.
2. Initialize a new instance of it into MiniProfiler.Settings.Storage. In an MVC application, this can be done in the `App_Start/MiniProfiler.PreStart()` method.

`MiniProfiler.Settings.Storage = new AzureCacheStorage(TimeSpan.FromHours(1));`
