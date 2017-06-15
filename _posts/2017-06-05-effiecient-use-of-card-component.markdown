---
layout: post
title:  Efficient use of event card component on open-event-front end

date:   2017-05-16 03:33:34 +0530
---

Ember JS is a powerful framework when it comes to code reusability. Compoenents are at it's core and enable the developers to re-use the same code at different places.
The `event-card` component on Open-event-front end is one such component. It is used on various routes across the app therby illustrating the usefulness. However, in this article we are going to see how components can be made even more efficient by rendering them differently for different situations.

Originally the component was used to display events in the card format on the public page.
And this was the code :

{% highlight html %}
{% raw %}
<div class="ui fluid event card">
  <a class="image" href="{{href-to 'public' event.identifier}}">
    {{widgets/safe-image src=(if event.large event.large event.placeholderUrl)}}
  </a>
  <div class="content">
    <a class="header" href="{{href-to 'public' event.identifier}}">
      <span>{{event.name}}</span>
    </a>
    <div class="meta">
      <span class="date">
        {{moment-format event.startTime 'ddd, MMM DD HH:mm A'}}
      </span>
    </div>
    <div class="description">
      {{event.shortLocationName}}
    </div>
  </div>
  <div class="extra content small text">
    <span class="right floated">
      <i role="button" class="share alternate link icon" {{action shareEvent event}}></i>
    </span>
    <span>
      {{#each tags as |tag|}}
        <a>{{tag}}</a>
      {{/each}}
    </span>
  </div>
</div>
{% endraw %}
{% endhighlight %}

Next we modify it in such a way that it is suitable to be displayed on the `explore` route as well, and that requires it to be such that is instead of being box-like it should be possible to render it such that it is wide and takes the width of the container it is in. 
How do we determine which version should be rendered when, so we pass an additional parameter while calling the component : `isWide` 

And the code would be something like this with `isWide`taken into account: 
{% highlight html %}
{% raw %}
<div class="{{if isWide 'event wide ui grid row'}}">
 
<div class="ui fluid event card">
  <a class="image" href="{{href-to 'public' event.identifier}}">
    {{widgets/safe-image src=(if event.large event.large event.placeholderUrl)}}
  </a>
  <div class="content">
    <a class="header" href="{{href-to 'public' event.identifier}}">
      <span>{{event.name}}</span>
    </a>
    <div class="meta">
      <span class="date">
        {{moment-format event.startTime 'ddd, MMM DD HH:mm A'}}
      </span>
    </div>
    <div class="description">
      {{event.shortLocationName}}
    </div>
  </div>
  <div class="extra content small text">
    <span class="right floated">
      <i role="button" class="share alternate link icon" {{action shareEvent event}}></i>
    </span>
    <span>
      {{#each tags as |tag|}}
        <a>{{tag}}</a>
      {{/each}}
    </span>
  </div>
</div>
{% endraw %}
{% endhighlight %}


Next What we are going to do is, modify the component  to become yieldable. So that they can also be used to display the tickets of a user! 
`{% raw } {{yield}} {% endraw %}` allows code outside the component to be rendered inside it. 

Let's make a change so that, if the event card component is rendered on the my tickets page, then instead of hashtags it should display the ticket details. Which we will conveniently provide to the component externally (via `{% raw %} {{yield}} {% endraw %}`)
Next we need to determine which version of the component should be rendered when?
The `hasBlock` helper enables us to do just that.
So the final code should look something just like this ;) 
{% highlight html %}
{% raw %}
<div class="{{if isWide 'event wide ui grid row'}}">
  {{#if isWide}}
    {{#unless device.isMobile}}
      <div class="ui card three wide computer six wide tablet column">
        <a class="image" href="{{href-to 'public' event.identifier}}">
          {{widgets/safe-image src=(if event.large event.large event.placeholderUrl)}}
        </a>
      </div>
    {{/unless}}
  {{/if}}
  <div class="ui card {{unless isWide 'event fluid' 'thirteen wide computer ten wide tablet sixteen wide mobile column'}}">
    {{#unless isWide}}
      <a class="image" href="{{href-to 'public' event.identifier}}">
        {{widgets/safe-image src=(if event.large event.large event.placeholderUrl)}}
      </a>
    {{/unless}}
    <div class="main content">
      <a class="header" href="{{href-to 'public' event.identifier}}">
        <span>{{event.name}}</span>
      </a>
      <div class="meta">
        <span class="date">
          {{moment-format event.startTime 'ddd, MMM DD HH:mm A'}}
        </span>
      </div>
      <div class="description">
        {{event.shortLocationName}}
      </div>
    </div>
    <div class="extra content small text">
      <span class="right floated">
        <i role="button" class="share alternate link icon" {{action shareEvent event}}></i>
      </span>
      <span>
        {{#if hasBlock}}
          {{yield}}
        {{else}}
          {{#each tags as |tag|}}
            <a>{{tag}}</a>
          {{/each}}
        {{/if}}
      </span>
    </div>
  </div>
</div>

{% endraw %}
{% endhighlight %}

