---
layout: post
title:  Creating a notification dropdown in semantic UI

date:   2017-05-16 03:33:34 +0530
---

Semantic UI is a modern front-end development framework which helps build responsive and aesthetically beautiful layouts. It comes pack with highly responisve componenets to cater to all front end needs, and provides excellent documentation for them. 
The web of front end development is so large, it is never possible to cover all the possible requirements of a developer with pre built components.

In this article we are going to build a notification dropdown from scratch.

Open event front end is a project of fossasia organisation, which was created with the aim of decoupling the front end and the back end for the open event orga server. It is primarily based on ember JS and uses semantic UI for 
Here we will be making a nested route 
`/events/` with 
`/events/live/`, `events/draft`, `events/past` , `events/import` as it's subroutes.

To get started with it, we simply use the ember CLI to generate the routes
{% highlight css %}
$ ember generate route events
{% endhighlight %}

Then we go on to generate the successive sub routes as follows
{% highlight css %}
$ ember generate route events/live
$ ember generate route events/past
$ ember generate route events/draft
$ ember generate route events/import
{% endhighlight %}

The `router.js` file should be looking like this now.
{% highlight java script %}
this.route('events', function() {
    this.route('live');
    this.route('draft');
    this.route('past');
    this.route('import');
  });
{% endhighlight %}

This means that our routes and sub routes are in place.
Since we used the emebr CLI to generate these routes, the template files for them would have generated automatically.

Next, we go to the template file of events/ route
which is at `templates/events.hbs`
And write the following code to create a menu and use ember integration of semantic UI `link-to` to link the tabs of the menu with the correspondinf correct route. It will take care of selecting the appropriate data for the corresponding route and display it in the correct tab via the outlet.

{% highlight html %}
{% raw %}
<div class="row">
  <div class="sixteen wide column">
    <div class="ui fluid pointing secondary menu">
      {{#link-to 'events.live' class='item'}}
        {{t 'Live'}}
      {{/link-to}}
      {{#link-to 'events.draft' class='item'}}
        {{t 'Draft'}}
      {{/link-to}}
      {{#link-to 'events.past' class='item'}}
        {{t 'Past'}}
      {{/link-to}}
      {{#link-to 'events.import' class='item'}}
        {{t 'Import'}}
      {{/link-to}}
    </div>
  </div>
</div>
<div class="ui segment">
  {{outlet}}
</div>
{% endraw %}
{% endhighlight %}

So finally, we start filling in the data for each of these routes. Let's fill some dummy data at 
`templates/events/live.hbs`
{% highlight html %}
{% raw %}
<div class="row">
  <div class="sixteen wide column">
    <table class="ui tablet stackable very basic table">
      <thead>
        <tr>
          <th>{{t 'Name'}}</th>
          <th>{{t 'Date'}}</th>
          <th>{{t 'Roles'}}</th>
          <th>{{t 'Sessions'}}</th>
          <th>{{t 'Speakers'}}</th>
          <th>{{t 'Tickets'}}</th>
          <th>{{t 'Public URL'}}</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>
            <div class="ui header weight-400">
              <img src="http://placehold.it/200x200" alt="Event logo" class="ui image">
              Sample Event
            </div>
          </td>
          <td>
            March 18, 2016 - 09:30 AM
            <br>(to)<br>
            March 20, 2016 - 05:30 PM
          </td>
          <td>
            <div class="ui ordered list">
              <div class="item">sample@gmail.com ({{t 'Organizer'}})</div>
              <div class="item">sample2@gmail.com ({{t 'Manager'}})</div>
            </div>
          </td>
          <td>
            <div class="ui list">
              <div class="item">{{t 'Drafts'}}: 0</div>
              <div class="item">{{t 'Submitted'}}: 0</div>
              <div class="item">{{t 'Accepted'}}: 0</div>
              <div class="item">{{t 'Confirmed'}}: 0</div>
              <div class="item">{{t 'Pending'}}: 0</div>
              <div class="item">{{t 'Rejected'}}: 0</div>
            </div>
          </td>
          <td>
            2
          </td>
          <td>
            <div class="ui bulleted list">
              <div class="item">{{t 'Premium'}} (12/100)</div>
              <div class="item">{{t 'VIP'}} (10/15)</div>
              <div class="item">{{t 'Normal'}} (100/200)</div>
              <div class="item">{{t 'Free'}} (100/500)</div>
            </div>
          </td>
          <td>
            <div class="ui link list">
              <a class="item" target="_blank" rel="noopener" href="http://nextgen.eventyay.com/e/ecc2001a">
                http://nextgen.eventyay.com/e/ecc2001a
              </a>
            </div>
          </td>
          <td class="center aligned">
            <div class="ui vertical compact basic buttons">
              {{#ui-popup content=(t 'Edit event details') class='ui icon button'}}
                <i class="edit icon"></i>
              {{/ui-popup}}
              {{#ui-popup content=(t 'View event details') class='ui icon button'}}
                <i class="unhide icon"></i>
              {{/ui-popup}}
              {{#ui-popup content=(t 'Delete event') class='ui icon button'}}
                <i class="trash outline icon"></i>
              {{/ui-popup}}
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

{% endraw %}
{% endhighlight %}

Similarly we can fill the required data for each of the routes.

And this is it, our nested route is ready.
Here are some screenshots.
