---
layout: home
title: "Getting started"
---

# Getting started

Jasmine-species provides extended BDD grammar and reporting for 
[Jasmine](http://pivotal.github.com/jasmine/). The grammar component 
enables you to go beyond "describe" and "it" when creating 
specifications for your product. The reporting component contains 
a StyledHtmlReporter that outputs (allegedly) "nicer" looking specs. 


## Features

**Alternate grammar to describe the behavior of your software:**

* [Feature/Story](docs.html#featurestory_grammar)
* [Given, When, Then (GWT)](docs.html#given_when_then_gwt_grammar)
* [Context/Specification](docs.html#contextspecification_grammar)

**Attach useful metadata to your suites and specs:**

* [Summary, Details](docs.html#metadata_grammar) 


## Requirements

To use jasmine-species you simply include it into your spec runner after 
you have included jasmine. Then import the grammar elements you need 
into your global namespace and you are ready to start using alternative 
grammar to describe your software.

You should have the items below in your spec runner to use jasmine-species.

* [Jasmine](http://pivotal.github.com/jasmine/) herself
* Any decent javascript namespace/package importer

    - Currently tested with [Namespacedotjs](https://github.com/smith/namespacedotjs)

    - You could try [AJILE](http://ajile.net/)


## Usage -- basic steps to get started with jasmine-species

Jasmine Species provides two components that extend the base Jasmine API.

The first is the **grammar** component which is the home of the extended 
BDD grammar. To use this component,

* include the "jasmine-grammar.js" file in your spec runner.
* then import only the grammar components you need e.g. Namespace.use('jasmine.grammar.FeatureStory.*')

The second components is the reporting component which outputs your spec 
reports with rich metadata for ease of use and customization. To experience 
the cleaner and richer ouput available from this component,

* include the "jasmine-reporting.js" file in your spec runner.
* then register the StyledHtmlReporter with Jasmine.

You can follow the steps below to start enjoying Jasmine Species.

1. Download and include the required libraries, Namespace.js and Jasmine in your runner

{% highlight html %}
...

<script type="text/javascript" src="lib/namespacedotjs/Namespace.js"></script>

<link rel="stylesheet" type="text/css" href="lib/jasmine/jasmine.css">
<script type="text/javascript" src="lib/jasmine/jasmine.js"></script>
<script type="text/javascript" src="lib/jasmine/jasmine-html.js"></script>

...
{% endhighlight %}

2. Download and include the grammar and reporting components from the jasmine-species package 

{% highlight html %}
...

<link rel="stylesheet" type="text/css" href="lib/jasmine-species/calm.css">
<script type="text/javascript" src="lib/jasmine-species/jasmine-grammar.js"></script>
<script type="text/javascript" src="lib/jasmine-species/jasmine-reporting.js"></script>

...
{% endhighlight %}

3. Import the extended grammar you wish to use

{% highlight javascript %}
// import feature, scenario ...
Namespace.use('jasmine.grammar.FeatureStory.*');
Namespace.use('jasmine.grammar.GWT.*');
Namespace.use('jasmine.grammar.Meta.*');
{% endhighlight %}

4. Setup your jasmine runner to use the StyledHtmlReporter

{% highlight javascript %}
var jasmineEnv = jasmine.getEnv();
var styledReporter = new jasmine.reporting.StyledHtmlReporter();

jasmineEnv.addReporter(styledReporter);

jasmineEnv.specFilter = function(spec) {
    return styledReporter.specFilter(spec);
};

window.onload = function() {
    jasmineEnv.execute(); 
}
{% endhighlight %}

5. Describe the features and behavior of your product

{% highlight javascript %}
feature('Car engine startup', function() {
    summary(
        'In order to drive my car around',
        'As a vehicle owner',
        'I want to press a button to start my car'
    );
    
    scenario('The is stopped with the engine off', function() {
        var car;
        
        given('My car is parked and not running', function() {
            car = new MyAwesomeCar();
        });
        when('I press the start button', function() {
            car.start();
        });
        then('The car should start up', function() {
            expect(car).toBeRunning();
        });
    });
});
{% endhighlight %}

Note: in the example above, we assume that "toBeRunning" is a custom 
matcher we wrote that does some quick checks against the api of a car 
to ensure that it is running.