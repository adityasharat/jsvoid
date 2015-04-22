A few months ago I had to develop some new feature for our Dashboard at [AppIterate](http://appiterate.com). The user had to create a set of rules based on numerous attributes, properties, values and conditions. These vectors could easily number in the 100s. So there were multiple drop downs where the user could select these vectors.

Searching through so many options on every drop down was becoming very tedious task for the users. After some research it was decided that we would use a jQuery pulgin [Chosen](http://harvesthq.github.io/chosen/). Chosen is a jQuery plugin that makes long, unwieldy select boxes much more user-friendly. It enable the user to search through options using a filter.

Here is how it worked:

<div data-height="256" data-theme-id="14343" data-slug-hash="mJdJRK" data-default-tab="js" data-user="adityasharat" class='codepen'><pre><code>$(&#x27;#people&#x27;).chosen();</code></pre>
<p>See the Pen <a href='http://codepen.io/adityasharat/pen/mJdJRK/'>mJdJRK</a> by Aditya Sharat (<a href='http://codepen.io/adityasharat'>@adityasharat</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
</div><script async src="//assets.codepen.io/assets/embed/ei.js"></script>

> doesn't work on mobile

so here is an image :)

{<1>}![](/content/images/2015/04/chosen-example.png)

Imagine doing that for every drop down in your application. Fortunately we were using [AngularJS](https://angularjs.org/) and we created a [Directive](https://docs.angularjs.org/guide/directive) that would make this plugin re-usable and awesome for our Dashboard.

Here is what I wanted to do:

```language-markup
<select chosen
  ng-model="data.country"
  ng-options="country.code as country.name for country in countries">
</select>
```

This was my first attempt:

```language-javascript
AngularChosen.directive('chosen', function() {

  // linker for the directive
  function linker($scope, iElm, iAttr) {
    iElm.chosen({
      width: '100%'
    });
  }
  // return the directive
  return {
    name: 'chosen',
    restrict: 'A',
    link: linker
  };
});
```

And it did NOT work :(

<p data-height="266" data-theme-id="14343" data-slug-hash="RPwWwd" data-default-tab="result" data-user="adityasharat" class='codepen'>See the Pen <a href='http://codepen.io/adityasharat/pen/RPwWwd/'>RPwWwd</a> by Aditya Sharat (<a href='http://codepen.io/adityasharat'>@adityasharat</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

It did not work because the `linker` executed before the `ng-options` directive. So the drop down was converted to chosen before the list of options was populated and the no one told that instance of the pulgin that it had to updated itself.

To fix this issue we have to `watch` the collection of options and update the plugin when the collection get populated or changes.

here are the changes that need to be made:

In the markup add a new attribute `options` to watch the collection of options.
```language-markup
<select chosen
  options="countries"
  ng-model="data.country"
  ng-options="country.code as country.name for country in countries">
</select>
```

In the directive we need to watch for changes to `options` as well as `ng-model`.
```language-javascript
function linker($scope, iElm, iAttr) {
  iElm.chosen();
  
  // watch the ngModel and options
  // trigger the chosen:updated event when it changes
  $scope.$watchCollection(['ngModel', 'options'], function () {
    iElm.trigger('chosen:updated');
  });
}

return {
  name: 'chosen',
  scope: {	// isolate scope 
    ngModel: '=',
    options: '='
  },
  restrict: 'A',
  link: linker
};
```

And now it works :)

<p data-height="268" data-theme-id="14343" data-slug-hash="rVNVJM" data-default-tab="result" data-user="adityasharat" class='codepen'>See the Pen <a href='http://codepen.io/adityasharat/pen/rVNVJM/'>rVNVJM</a> by Aditya Sharat (<a href='http://codepen.io/adityasharat'>@adityasharat</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

There are many more events and corner case that needed to be handled before I used this in production. You can find the complete implmentation [here](https://github.com/adityasharat/angular-chosen/blob/master/angular-chosen.js) and a complete demo at [GitHub](http://adityasharat.github.io/angular-chosen/)

Please let know what you think of this solution. I would love to find out if there is a right way or a better way to do it. Point out if I did something wrong. All comments are welcome. :)








