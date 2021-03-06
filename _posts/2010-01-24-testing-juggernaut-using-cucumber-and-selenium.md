---
layout: post
title: "Testing Juggernaut using Cucumber and Selenium"
redirect_from:
  - /testing-juggernaut-using-cucumber-and-selenium
  - /testing-juggernaut-using-cucumber-and-selenium/
  - /blog/2010/01/testing-juggernaut-using-cucumber-and-selenium
  - /blog/2010/01/testing-juggernaut-using-cucumber-and-selenium/
---

During tests development you usually use well-known and tested solutions like [RSpec][rspec] or [Cucumber][cucumber] + [Webrat][webrat]. They let you develop in a fast and convinient way. Unfortunately there are times when you need to get you hands dirty. If your application use only javascript then [Celerity][celerity] may help. But as soon as you incorporate Flash or Java into your application things get much harder. For such situations the [Selenium][selenium] framework should be well-suited. It lets you test your app by simulating user actions through any (supported) web browser. Connection of Selenium and Cucumber seem to be very interesting option too - for more details look at Cucumber wiki.

By default Cucumber uses one browser to open your application and perform selected tests. But from time to time you may need more instances to run in parallel mode - e.g to test real-time user communication. Luckily there is an easy way to override Selenium scripts in Cucumber letting you open another browser instance and switch between them. You just need to add following lines into "env.rb" (or selenium dedicated configuration file).

{% highlight ruby %}
$browser2 = Webrat::SeleniumSession.new.selenium
$browser = nil
$browser1 = Webrat::SeleniumSession.new.selenium
{% endhighlight %}

The second line is necessary to proper initialization of the second browser by Cucumber. After that, you just need to write appropriate steps for Selenium, say:

{% highlight ruby %}
Given /^I am using first browser$/ do
  if Webrat.configuration.mode == :selenium
    $browser = $browser1
  end
end

Given /^I am using second browser$/ do
  if Webrat.configuration.mode == :selenium
    $browser = $browser2
  end
end
{% endhighlight %}

By default the first browser will be active one. But you should not forget that lastly used browser is not restarted between two consecutive tests. You should always switch to the first browser at the end of your test. In addition, there are two useful steps:

{% highlight ruby %}
When /^I wait for page to load$/ do
  if Webrat.configuration.mode == :selenium
    selenium.wait_for_page_to_load
  end
end

Given /^I wait for juggernaut$/ do
  if Webrat.configuration.mode == :selenium
    sleep(3)
  end
end
{% endhighlight %}

First one ensures that all scripts are loaded before moving on to the next steps. Second one gives Juggernaut time to deliver messages. With this configuration you can use all delivered by Cucumber steps without worrying about compatibility problems.

[celerity]: https://celerity.rubyforge.org
[cucumber]: https://cukes.info
[rspec]: https://rspec.info
[selenium]: http://seleniumhq.org
[webrat]: https://github.com/brynary/webrat
