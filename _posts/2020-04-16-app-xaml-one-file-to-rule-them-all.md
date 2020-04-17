---
layout: post
title: App.xaml - One file to rule them all
excerpt: "<strong>App.xaml</strong> is your application main file and is automatically
  created. It define what your app will do when starded, suspended, onbackground,
  etc.<br><br>When created, the file look something like this"
date: 2020-04-16 03:00:00 +0000
last_modified_at: 2020-04-16 03:00:00 +0000
categories: []
tags: []
comments: false

---
### Intro

**App.xaml** is your application main file and is automatically created. It define what your app will do when starded, suspended, onbackground, etc.

When created, the file look something like this

{% highlight c# %}

    <Application x:Class="MySolution.App"
    	xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    	xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    	StartupUri="MainWindow.xaml">
                
    	<Application.Resources>
                
    	</Application.Resources>
        
       </Application>

{% endhighlight %}

Let's explore the main features that we can use in this file.

### Properties

#### StartupUri

The most important property of an application is StartupUri, this prop specify the UI that automatically opens when an application starts. So, if you do

{% highlight c# %}

    Application x:Class="MySolution.App"
    	xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    	xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    	StartupUri="Biscoito.xaml">
                
    	<Application.Resources>
                
    	</Application.Resources>
    </Application>

{% endhighlight %}

_Biscoito.xaml_ will open when you run the app.

You can see the full list of superted UIs that can be set on StartupUri [here](https://docs.microsoft.com/en-us/dotnet/api/system.windows.application.startupuri?view=netframework-4.8).

#### Resources

With this property we can set a collection of application-scope resources, such as styles, brushes and converters. It's great to create a general theme for the app, being thread safe and avaible from any thread.

We can set a Resource from code-behin

{% highlight c# %}

    Application.Current.Resources["ApplicationScopeResource"] = Brushes.White;

{% endhighlight %}

Or throuth the .xaml

{% highlight c# %}

    <Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    	xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    	x:Class="MyApp.App"
    	StartupUri="MainWindow.xaml">
    	
    	<Application.Resources>
    		<SolidColorBrush x:Key="MyWhite" Color="White"></SolidColorBrush>
    	</Application.Resources>
    </Application>

{% endhighlight %}

eitherway the solid brush will be avaible for all the assembly.

We can also get the resource via code-behind

{% highlight c# %}

    Brush whiteBrush = (Brush)Application.Current.Resources["MyWhite"];

{% endhighlight %}

or .xaml

{% highlight c# %}

    <TextBlock Text="Biscoito" Foreground="{StaticResource MyWhite}"/>

{% endhighlight %}

Resources are a good way to implement a theme for the app, 'cause if resources change, the resource system ensures that element properties which are bound to those resources are automatically updated to reflect the change.

### Events

One of the most cool things on c# are events, and when talking about the Application class we cannot go on whitout talking about them. Here we will see the 3 most interesting ones

* Startup;
* DispatcherUnhandledException;
* Exit.

#### Startup

This event is callend when the [Run()](https://docs.microsoft.com/en-us/dotnet/api/system.windows.application.run?view=netframework-4.8#System_Windows_Application_Run) method is invoked; thefore, after the application execution model has been established.

Startup can be of great value when dealing with parameters or if we whant to define some properties and/or datacontext of our main window before it is inicialized.

The [documentation](https://docs.microsoft.com/en-us/dotnet/api/system.windows.application.startup?view=netframework-4.8) has one good example illustrating parameters handling.

#### DispatcherUnhandledException

This event is called whenever an exception is throw by the main UI thread. So subscribing to it let we handle the exceptions we are not expected to hapen (not used a try/catch)

It's important to notice that if the exceptions are throw by a background UI thread or a background worker it will never get through the main UI thread.

So, once we subscribe to the event.

{% highlight c# %}

    <Application x:Class="WpfTutorialSamples.App"
    	xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    	xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    	DispatcherUnhandledException="Application_DispatcherUnhandledException"
    	StartupUri="MainWindow.xaml">
    	
    	<Application.Resources>
    	
    	</Application.Resources>
    	
    </Application>

{% endhighlight %}

We just need to implement how we whant to handle the exception.

{% highlight c# %}

    namespace MySolution 
    {
    	public partial class App : Application 
    	{
    		private void Application_DispatcherUnhandledException(object sender, DispatcherUnhandledExceptionEventArgs e) 
    		{
    			MessageBox.Show($"An unhandled exception just occurred: {e.Exception.Message}", "Exception Sample", MessageBoxButton.OK, MessageBoxImage.Warning);
    			e.Handled = true;
    		}
    	}
    }

{% endhighlight %}

You could ask why we set \`e.Handled = true\` and would be a good question! If you look at the [documentation](https://docs.microsoft.com/en-us/dotnet/api/system.windows.application.dispatcherunhandledexception?view=netframework-4.8) you will`read that when a`unhandled exeption is throw the Windows Run Time will close the app unless that the exeption is setted as handle.

Be awere that some times we have no choice but let the WRT kill the aplication. We can decide that looking in the parameter; e.Exception will say if the exception was just a naive FileNotFoundException or an unrecovable StackOverflowException.

#### Exit

Occurs just before an application shuts down, and cannot be canceled. It's usefull when we whant to do diferent things depending on the reason the app was closed.

For example, if the aplication is closing because os an exeption, we can send an email with the crash logs or save the state of the app and, overriding the OnLaunched, recover the state once the app is restardted.

### Documentation

You should also read the [full documentation](https://docs.microsoft.com/en-us/dotnet/api/system.windows.application?view=netframework-4.8).