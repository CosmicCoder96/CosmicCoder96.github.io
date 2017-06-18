---
layout: post
title:  Using ember semantic ui radio buttons to render form elements selectively.

date:   2017-06-18 05:33:34 +0530
---

This blog article will illustrate how to to use ember semantic ui radio buttons to render form elements selectively and in the process will learn, how to make use of the powerful features offered by ember semantic ui for radio buttons via the `mut` action.


So what do we have to begin with ?

The sample form which we want to create
`Image of the form`
What we want is that the radio button should allow us to make a choice and then display the curresponding fields. Now that seems a trivial process, but there is some thought process which goes into this, to end up with the most efficient choice. We will also discuss some of the obvious things that may come to mind to complete this task.
So first let's make the basic form where in all the fields are visible.

{% highlight html %}
{% raw %}
    <h3 class="ui header">{{t 'PayPal Credentials'}}</h3>
    <h5 class="ui header">{{t 'PayPal Integration Mode'}}</h5>
    <div class="field">
      {{ui-radio label=(t 'Sandbox mode - Used during development and testing') name='paypal_integration_mode'  value='sandbox'  current='sandbox' onChange=(action (mut selectedMode))}}
    </div>
    {{#unless (eq selectedMode 'live')}}
      <div class="field">
        <label>{{t 'Sandbox username'}}</label>
        {{input type='text' name='sandbox_username'}}
      </div>
      <div class="field">
        <label>{{t 'Sandbox password'}}</label>
        {{input type='password' name='sandbox_password'}}
      </div>
      <div class="field">
        <label>{t 'Sandbox signature'}}</label>
        {{input type='text' name='sandbox_signature'}}
      </div>
    {{/unless}}
    <div class="field">
      {{ui-radio label=(t 'Live mode - Used during production') name='paypal_integration_mode' value='live' onChange=(action (mut selectedMode))}}
    </div>
    {{#if (eq selectedMode 'live')}}
      <div class="field">
       <label>{{t 'Live username'}}</label>
        {{input type='text' name='live_username'}}
      </div>
      <div class="field">
        <label>{{t 'Live password'}}</label>
        {{input type='password' name='live_password'}}
      </div>
      <div class="field">
        <label>{{t 'Live signature'}}</label>
        {{input type='text' name='live_signature'}}
      </div>
    {{/if}}
    <button class="ui teal button" type="submit">
    {{t 'Save'}}
    </button>
{% endraw %}
{% endhighlight %}
