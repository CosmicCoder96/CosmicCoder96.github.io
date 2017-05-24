---
layout: post
title:  Creating a notification dropdown in semantic UI

date:   2017-05-16 03:33:34 +0530
---

Semantic UI is a modern front-end development framework which helps build responsive and aesthetically beautiful layouts. It comes pack with highly responisve componenets to cater to all front end needs, and provides excellent documentation for them. 
The web of front end development is so large, it is never possible to cover all the possible requirements of a developer with pre built components.

In this article we are going to build a notification dropdown from scratch.
So we begin by generating a new component via `ember CLI` 
{% highlight css %}
$ ember generate component notification-dropdown 
{% endhighlight %}

This should generate the boiler plate code for our component, with the template file located at: 
`templates/components/notification-dropdown.hbs`
It is assumed that you already have a basic ember app with atleast a navbar set up. The notification drop down will be integrated with the navbar as a separate component.

We will use the `popup` component of semantic ui as the ubderlying structure of our dropdown. 
I have used some dummy data stored in a separate file, you can use any dummy data you wish, either by directly hardcoding it or importing it from a js file stored somewhere else.

We will make use of the `floating label` of semantic UI to display the number of unread notifications.


{% highlight html %}
{% raw %}
  <i class="mail outline icon">
</i>
<div class="floating ui teal circular mini label">{{notifications.length}}</div>
<div class="ui wide notification popup bottom left transition ">
  <div class="ui basic inverted horizontal segments">
    <div class="ui basic left aligned segment weight-800">
      <p>{{t 'Notifications'}}</p>
    </div>
    <div class="ui basic right aligned segment weight-400">
      <a href="#">{{t 'Mark all as Read'}}</a>
    </div>
  </div>
  <div class="ui fluid link celled selection list">
    {{#each notifications as |notification|}}
      <div class="item">
        <div class="header">
          {{notification.title}}
        </div>
        <div class="content weight-600">
          {{notification.description}}
        </div>
        <div class="left floated content">
          {{moment-from-now notification.createdAt}}
        </div>
      </div>
    {{/each}}
  </div>
</div>

{% endraw %}
{% endhighlight %}

Now the next biggest challenge is to make the popup scrollable, they are not scrollable by default and may result in an error if their height exceeds that of the view port. So we apply some styling now.

{% highlight css %}
.notification.item {
  margin: 0 !important;
  .label {
    top: 1em;
    padding: 0.2em;
    margin: 0 0 0 -3.2em !important;

  }
}

.ui.notification.popup {
  padding: 2px;
  .list {
    width: auto;
    max-height: 50vh;
    overflow: hidden;
    overflow-y: auto;
    padding: 0;
    margin: 0;
    .header {
      margin-bottom:5px;
    }
    .content {
      margin-bottom:2px;
    }
  }
  .segments {
    margin: 5px;
  }
  .segment {
    border: none;
    padding: 0 2px;
    margin: 0;
  }
}

{% endhighlight %}

All of this takes care of the styling.
Next, we need to take care of initialising the notification popup. For this we need to go to the navbar component as it is the one who calls the notification dropdown component. And add this to it:

{% highlight java script %}
didInsertElement() {
    this._super.call(this);
    this.$('.notification.item').popup({
      popup : '.popup',
      on    : 'click'
    });
  },

  willDestroyElement() {
    this._super.call(this);
    this.$('.notification.item').popup('destroy');
  }
{% endhighlight %}

This will take care of cleaning up as well.
Now, you should have the notification drop down up and running !! 
