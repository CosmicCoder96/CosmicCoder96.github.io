---
layout: post
title:  emberJS - when is it the right choice
date:   2017-05-09 03:33:34 +0530
---

With the plethora of java script frameworks available, it gets really diffciult to decide, which one is actually the right choice for a project, especially for the not so experienced developers who are just starting out. Every month a new framework arrives, and the exisiting ones keep activiely updating themselves often.  This article covers one such framework **emberJS**  and highlights it's advantages over others and with an example demostorates it's usefulness.

<figure>
	<img src="/images/ember-logo.png" style="width:300px">
</figure>

EmberJS is an open-source JavaScript application framework for creating single-page client-side web applications, which uses Model-View-Controller (MVC) pattern. The framework provides universal data binding together and URL-driven approach for structuring different applications with the focus on scalability.


<h2>Why is Ember JS great?</h2>
<h3> Convention over configuration - It does all the heavy lifitng. </h3>
Ember JS  mandates best practices, enforces naming conventions and generates the boiler plate code for the various components and routes itself. This has advantages other than uniformity. It is easier for other developers to join the project and start working right away, instead of spending hours on existing codebase to understand it, as the core structure of all ember apps is similar. To get an ember app started with the basic route, user doesn't has to do much, ember does all the heavy lifting.
{% highlight css %}
	$ ember new my-app
	$ ember server
{% endhighlight %}
After installing this is all it takes to create your app.

<h3>Ember CLI </h3>
Similar to Ruby on Rails, ember has a powerful CLI. It can be used to generate boiler plate codes for components, routes, tests and much more. Testing is possible via the CLI as well.
{% highlight css %}
$ ember generate component my-component
$ ember generate route myRoute
$ ember test
{% endhighlight %}
These are some of the examples which show how easy it is to manage the code via the emebr CLI.
<h3>Tests.Tests.Tests.</h3>
Ember JS makes it incerdibly easy to use test-first approach. Integration tests, acceptance tests, and unit tests are in built into the framework. And can be geerated from the CLI itself, the documenration on them is well written and it's really easy to customise them.
{% highlight css %}
$ ember g acceptance-test your test
{% endhighlight %}
This is all it takes to set up the entire boiler plate for the test, which you can customise
<h3>Excellent documentation and guides</h3>
Ember JS has one of the best possible documentations available for a framework. The guides are a breeze to follow. It is highly recommended that, if starting out on ember, make the demo app from the official ember Guides. That should be enough to get familiar with ember.

<h3> Ember Data </h3>
It sports one of the best implemented API data fetching capabilities. Fetching and using data in your app is a breeze.
<h3>Where is it being used?</h3>
Ember has a huge community and is being used all around. This article focuses on it's salien features via the example of Open Event Orga Server project of FOSSASIA. The unorga server is primarily based on FLASK with jinja2 being used for rendering templates. At the small scale, it was efficient to have both the front end and backend of the server together, but as it grew larger in size with more refined features it became tough to keep track of all the minor edits and customizations of the front end and the code started to become complex in nature.
And that gave birth to the new project Open Event Front End which is based on ember JS which wil be covered in the next week.

With the orga server being converted into a fully functional API, the back end and the front end will be decoupled therby making the code much cleaner and easy to understand for the other developers that may wish to contribute in the future.
Also, since the new front end is being designed with ember JS, it's UI will have a lot of enhanced features and enforcing uniformity acorss the design would be much easier with the help of components in ember.
For instance, instead of making multiple copies of the same code, components are used to avoid repition and ensure uniformity (change in one place will refelct everywhere)
{% highlight html %}
{% raw %}
<div class="ui fluid vertical {{unless device.isMobile 'pointing'}} menu">
  <a class="item {{if (eq session.currentRouteName 'public.index') 'active'}}" href="{{href-to 'public.index'}}">
    {{t 'Info'\}}
  </a>
  <a class="item" href="{{href-to 'public.index'}}#tickets">
    {{t 'Tickets'}}
  </a>
  <a class="item" href="{{href-to 'public.index'}}#speakers">
    {{t 'Speakers'}}
  </a>
  {{#link-to 'public.sessions' class='item'}}
    {{t 'Sessions'}}
  {{/link-to}}
  {{#link-to 'public.schedule' class='item'}}
    {{t 'Schedule'}}
  {{/link-to}}
  {{#link-to 'public.cfs' class='item'}}
    {{t 'Call for Speakers'}}
  {{/link-to}}
  <a class="item" href="{{href-to 'public.index'}}#getting-here">
    {{t 'Getting here'}}
  </a>
</div>
{% endraw %}
{% endhighlight %}
This is a perfect example of the power of components in ember, this is a component which renders differently based on the screen size, therby reducing the redundancy of writing separate components for the same.

Ember is a step forward towards the future of the web. With the help of Babel.js it is possible to write ES6/2015 syntax and not worry about it's compatibility with the browsers. It will take care of it.

{% highlight java script %}
actions: {
    submit() {
      this.onValid(() => {

      });
    }
  }
{% endhighlight %}

This is perfectly valid and will be compatible with majority of the supported browsers.