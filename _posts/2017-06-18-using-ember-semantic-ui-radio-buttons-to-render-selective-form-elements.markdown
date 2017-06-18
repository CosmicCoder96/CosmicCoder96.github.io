---
layout: post
title:  Using ember semantic UI radio buttons to render form elements selectively.

date:   2017-06-18 05:33:34 +0530
---

This blog article will illustrate how to to use ember semantic ui radio buttons to render form elements selectively and in the process will learn, how to make use of the powerful binding features offered by ember semantic ui for radio buttons via the `mut` action.


So what do we have to begin with ?

The sample form which we want to create
<figure>
  <img src="/images/blog-6-1.png">
  <figcaption>A form which allows us to chose one of the modes of paypal payments and displays corresponding fields for it.</figcaption>
</figure>
What we want is that the radio button should allow us to make a choice and then display the curresponding fields. Now that seems a trivial process, but there is some thought process which goes into this, to end up with the most efficient choice. We will also discuss some of the obvious things that may come to mind to complete this task.
So first let's make the basic form where in all the fields are visible.

Now let's learn how to make use of the `mut` action on the radio buttons. 
What it allows us to do is pass a parameter while calling it, and that parameter name is shared by all the radio buttons belonging to a particular group of radio buttons.
And what that action does is, store the name of the currently selected radio button in it. So we can easily keep track of which button has been selected and use that variable in selective rendering of templates.
The action is triggered whenever the radio button's property changes and the trigger is aptly called `onChange`.
So essentially the syntax boils down to this : 


{% highlight html %}
{% raw %}
<!-- The first radio button -->
{{ui-radio label=(t 'Sandbox mode - Used during development and testing') name='paypal_integration_mode'  value='sandbox' onChange=(action (mut selectedMode))}}
<!-- The second radio button -->
{{ui-radio label=(t 'Live mode - Used during production') name='paypal_integration_mode' value='live' onChange=(action (mut selectedMode))}}
{% endraw %}
{% endhighlight %}

Now whichever button is selected it's name will be stored in `selectedMode` in this case.
And hence we can use the conditional helpers of handle bars to render elements based on the selected radio button.

The final code looks something like this: 
{% highlight html %}
{% raw %}
    <h3 class="ui header">{{t 'PayPal Credentials'}}</h3>
     <div class="sub header">{{t 'See'}}<a href="https://developer.paypal.com/docs/classic/express-checkout/integration-guide/ECAPICredentials/?mark=lifecycle%20credentials" target="_blank" rel="noopener nofollow">{{t 'here'}}</a>
        {{t 'on how to obtain these keys.'}}
      </div>
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

### Important tip
The action is triggered by the `onChange` action, hence the variable doesn't have the value when the template is rendered for the very first time and hence at that instant, none of the fields will be rendered, to avoid that we have used both `if` and `unless` condition helpers instead of identical conditional helpers to cleverly avoid this situation. You can read about the ember semantic ui radio buttons further more through the [official documentation ](http://semantic-org.github.io/Semantic-UI-Ember/#/modules/radio)

