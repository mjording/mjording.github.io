---
layout: post
title: Eloquent JavaScript on Rails: Thinking Outside the DOM
---

## Eloquent JavaScript on Rails: Thinking Outside the DOM

Cyrus Innovation is a consulting firm that provides growing companies with the expert software development and agile reinforcement that they need to build more features, better products, and more productive teams.

**Taking Stock of Your “Front-End” Code**

We’re often called in to help when a company has a successful product built in Ruby on Rails and JavaScript that has become so complex over time that it’s difficult to maintain and extend.

A common source of complexity is the front-end code.  The JavaScript written for most Rails applications is still considered part of the view and not developed with the same rigorous standards as backend code. Yet in the lifecycle of most web application projects, the  “front-end” JavaScript codebase quickly matches backend in size if not complexity.

When assessing a new codebase, here are areas that we examine in order to come up with an inventory of trouble spots:

*   **Inline JavaScript**. Inline JavaScript increases download times for the page, avoids code modularity, and reduces ability to unit test.
*   **Constructors & classical inheritance**.  JavaScript is a prototypically composed language, so prototypical inheritance should be leveraged over classical.
*   **Feature tests checking JavaScript functionality**.  Although they give the stakeholder a modicum of confidence, feature tests do not test the delivered code, but rather only test the user experience. This means that although an interaction as an example of the feature is tested, the code that produces the functionality is not. Without fail, obfuscated and untested code results in unexpected behavior. Feature tests are not enough.
*   **Untested third party libraries**.  It’s easy for developers to reach for an off-the-shelf library that delivers UX enhancements for a feature request. The downside being a reduction in understanding the underlying implementation by responsible developers. The danger of which is exacerbated when the library in question is untested.



**Testing**

The first step to refactoring a troubled front-end is to establish some best practices around testing. Solid unit testing is essential for properly designed and well-composed code.

Unit testing is not the same as a Cucumber/Capybara user feature test or an automated Quality Assurance test. Giving precedence to feature tests can cause deeper problems on a project. For more information, research the [inverted automated](http://martinfowler.com/bliki/TestPyramid.html) [testing pyramid](https://www.mountaingoatsoftware.com/blog/the-forgotten-layer-of-the-test-automation-pyramid) / [ice cream cone](http://watirmelon.com/2012/01/31/introducing-the-software-testing-ice-cream-cone/). Also, J.B. Rainsberger’s talk, “[Integrated Tests Are A Scam](https://vimeo.com/80533536),” is a good resource.

**_Testing Tools_**

While all Rails developers should be familiar with ruby/Rails testing tools, such as RSpec, minitest, capybara, and others, some may not be familiar with good tools for testing JavaScript code.  We currently recommend [Jasmine](http://jasmine.github.io/) and [Teaspoon](https://github.com/modeset/teaspoon).

Jasmine is a standard for unit testing JavaScript. It maintains similar syntax as RSpec, Test::Unit, or minitest, adding some additional utility for mocking and stubbing.

Teaspoon is a test runner for JavaScript testing in Rails. It gives traditional Rails sugar to JavaScript testing. Teaspoon supports driving tests via a simple rake task: rake teaspoon. It also supports the standard Rails asset pipeline.

**_Writing JavaScript Unit Tests_**

Unit tests should limit their scope to test only the JavaScript function you write. Rather than testing third-party code, native browser functions, etc., you should leverage the mock/stub or test double utility of Jasmine called Spies.

For example, given:

```
var UserInformation = function() {   
  this.update = function(user_id, params) {
   jQuery.ajax({
          method: "PATCH", 
          url: "/user_information/"+user_id,
          data: params
   });   
  };
};
```

A proper unit test would spyOn the jQuery AJAX function:

```
//= require jquery
//= require user_information
describe("UserInformation", function() {
   describe("#update", function() {
       it("calls AJAX with the correct parameters and endpoint", function() {
           spyOn(jQuery, "ajax");
           var params = { "company": "Cyrus Innovation"};
           var user_information = new UserInformation();
           user_information.update(1, params);
           expect(jQuery.ajax).toHaveBeenCalledWith({
               method: "PATCH",
               url: "/user_information/1",
               data: params
           });
       });
   });
```
Now my JavaScript under test, UserInformation.js, is only exercised up to the point where it interfaces with jQuery.

**_Linting_**

We also recommend running a linter like [JSHint](http://jshint.com/), which applies a simple code style checker to enforce best practices in composition.

**_Next Steps_**

There are many other common areas of complexity in large Rails applications, such as bloated models or tangled view layers. We’d be happy to go on, but we’ve hit our space limit!

To chat with a Cyrusite about your codebase, product, or how you can [join our team](http://www.cyrusinnovation.com/careers/current-openings/), send a note to [info@cyrusinnovation.com](https://mail.google.com/mail/?view=cm&fs=1&tf=1&to=info@cyrusinnovation.com).

Happy Holidays from the Cyrus Team!

