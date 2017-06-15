---
layout: post
title:  Making currency name and symbol helpers for open event fronend

date:   2017-06-14 03:33:34 +0530
---

This blog article will illustrate how to make two helpers which will help us in getting the currency name and symbol from a dictionary, conveniently. It also exemplifies the power of ember and why is it being used in this project via a counter example in which we try to do things the non ember way and get the required data without using those helpers.

So what do we have to begin with ?

The sample data which will be fetched from the API:
{% highlight javascript %}
[
      {
        currency   : 'PLN',
        serviceFee : 10.5,
        maximumFee : 100.0
      },
      {
        currency   : 'NZD',
        serviceFee : 20.0,
        maximumFee : 500.0
      }
      //The list continues
]
{% endhighlight %}

The dictionary data format: 
{% highlight javascript %}
[
  {
    paypal : true,
    code   : 'PLN',
    symbol : 'zł',
    name   : 'Polish zloty',
    stripe : true
  },
  {
    paypal : true,
    code   : 'NZD',
    symbol : 'NZ$',
    name   : 'New Zealand dollar',
    stripe : true
  },
  {
    paypal : false,
    code   : 'INR',
    symbol : '₹',
    name   : 'Indian rupee',
    stripe : true
  }
]
// The list continues
{% endhighlight %}
And our primary goal is to fetch the corresponding name and symbol from the dictionary for a given currency code, easily and efficiently.

One might be tempted to get things done the easy way : 
via 

`{% raw %}{{get (find-by 'code' modal.name currencies) 'name'}}{% endraw %}`
and 
`{% raw %}{{get (find-by 'code' modal.name currencies) 'symbol'}}{% endraw %}`

where `currencies` is the name of the imported array from the dictionary.
But this might be hard to follow for a first time reader, and also in case we ever need this functionality to work in a different context, this is clearly not the most feasible choice.
Hence helpers come into picutre, they can be called anywhere and will have a much simpler syntax 

Our goal is to make helpers such that the required functionality is achieved with a simpler syntax than the one shown previously.


So, this is how they would look 
{% highlight javascript %}
{% raw %}
import Ember from 'ember';
import { find } from 'lodash';
import { paymentCurrencies } from 'open-event-frontend/utils/dictionary/payment';

const { Helper } = Ember;

export function currencyName(params) {
  return find(paymentCurrencies, ['code', params[0]]).name;
}

export default Helper.helper(currencyName);

{% endraw %}
{% endhighlight %}
 
And for the currency symbol helper 
{% highlight javascript %}
{% raw %}
import Ember from 'ember';
import { find } from 'lodash';
import { paymentCurrencies } from 'open-event-frontend/utils/dictionary/payment';

const { Helper } = Ember;

export function currencySymbol(params) {
  return find(paymentCurrencies, ['code', params[0]]).symbol;
}

export default Helper.helper(currencySymbol);
{% endraw %}
{% endhighlight %}
I
Now all we need to do use them is `{% raw %}{{currency-name 'USD'}}{% endraw %}`
and `{% raw%}{{currency-symbol 'USD'}}{% endraw %}`
to get the corrsepondong currency name and symbol.