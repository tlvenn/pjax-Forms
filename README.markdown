# P-JAX Integration in Play Framework 2.0 (Scala)


## What's this

This is a sample project that shows a possible way to integrate Pjax(https://github.com/defunkt/jquery-pjax) and Play 2.0.

Pjax is a jquery-based tool that loads HTML from the server into the current page without a full reload (via Ajax), updating the navigation bar and history (using HTML 5). It only works with browsers that support the history.pushState API. On unsupported browsers it simply uses standard redirects to the given URLs (aka links work normally).

This project uses the Forms sample project from Play 2.0 (Scala) as scaffolding and integrates Pjax to test both how it could be done and how much does Pjax improve the speed of the application.


## Any online demo?

Yes, you can see this project running on [Heroku](http://pjaxforms.herokuapp.com/)

Be aware it is running under a free dyno, so it may take a while to load the first time you access the application!

Begin by ensuring that [your browser is supported](http://caniuse.com/#search=pushstate)

The best way to compare the performance impact is to:

 - Load the site once (so Heroku's dyno is launched)
 - Launch the Firebug 'Net' plugin
 - Reload the main page again (F5) and check the loading time
 - Click on the header link 'Forms' (to load the main page using 'pjax') and check the time it takes

In my tests the main page (once the dyno is up) loads in around 1.5s while the pjax request takes around 250ms.

As you can see the difference is massive.


## What are the changes to the original project?

The following changes have been done to the original Forms Sample project:

1- Pjax library added and most anchor links configured to run using Pjax. The code includes two possible ways to enable Pjax.
2- All controller methods have 'request' declared as implicit.
3- All templates accept 'request' as an implicit parameter
4- The main template 'main.scala.html' has been modified to return a different HTML Response if we are in a Pjax request or not.
5- The implementation of the @title part has been modified in the templates, it has moved from the 'main' template to each specific template.

The reason behind the changes is that Pjax sends a specific header to identify a Pjax request. If that header is in the request we want to return only the code belonging to the page itself, without any layout. Otherwise we want the full render (layout + page requested).

As Play 2.0 templates work as nested functions that call to each other, we can only do that check in the 'main.scala.html' template, where we can decide to add or not to add the layout to the Response. But to do this, 'main' requires access to the 'request' object, which is not available by default, so we have to pass it as an implicit parameter. And because 'main' is used by all the templates in the application, all of them need to provide that 'request' object, and in turn the controllers that call the views must provide it to the views. As you can see, a bit invasive. If you know of a better way, please tell! :)

The changes to '@title' are specific to the way this application (Forms Sample) was behaving, by using the title to set some text in the body area which changes on each request. This has been done to keep the same layout as the original project.


# Client Side Validation

This project has been extended with a sample on client-side validation

The validation is provided by the [play-js-validation module](https://github.com/namin/play-js-validation)

## Code changes

Integrated client side validation in Play via [play-js-validation module](https://github.com/namin/play-js-validation)

Currently this module has some dependencies: https://github.com/js-scala/js-scala and https://github.com/js-scala/build-play20 , so it can't be used in Heroku.

Pending to see if it will be integrated in Play 2.1


# License (for those who need one)

Public Domain. Do whatever you want, at your own risk. No guarantees given.




