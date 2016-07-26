angular-autocomplete module
===========================

```
bower install git@gitlab.socrate.vsct.fr:dwm-sii/angular-autocomplete.git
npm install
grunt
```

Sources
-------
src
&nbsp;&lfloor; js/
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lfloor; autocomplete.module.js
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lfloor; autocomplete.directive.js
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lfloor; station-finder.service.js
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lfloor; translation.service.js
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lfloor; autocomplete.service.js &larr; where you will put your service
&nbsp;&lfloor; css/
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lfloor; angular-autocomplete.css

Needed files
------------
#### CSS
```
<!-- CSS -->
<link rel="stylesheet" type="text/css" href="dist/angular-autocomplete.css">
```
#### JavaScript
```
<!-- AngularJS tested versions -->
<script src="vendors/angular/angular.min.js"></script>
```
```
<!-- angular-autocomplete module (generated file) -->
<script src="dist/js/angular-autocomplete.js"></script>
```
```
<!-- Station Finder -->
<script src="http://integration1.stationfinder.voyages-sncf.com/widget/script/2?uc=FR_fr&onload=stfCallback" async defer></script>
```

Station finder callback
-----------------------
Station finder will be initiated by the station finder script itself once it will be loaded thanks to the function you specified in Station finder url ```&onload=stfCallback```
```
<script>
  var STF_WIDGET = null;
  function stfCallback() {
    STF_WIDGET = StationFinder.init('stfContainer');
  };
</script>
```

Your service
------------
You have two ways to call your own service:
1. Put it in **dist/js/autocomplete.service.js**
2. Or call an AngularJS function of your choice

#### js/autocomplete.service.js
You will put your own service in **dist/js/autocomplete.service.js**.
Then you'll have to run grunt again to update concatenated file **dist/js/angular-autocomplete.js**.
```
(function() {

  'use strict';

  function AutocompleteService () {
    return {
      get: function (search, callback) {
        // Your service goes here
        // You have to create an object that complies with this mock structure
        var results = [
          {
            "label": "firstList",
            "results": [
              {
                "country": "FR",
                "uicCode": "95027146",
                "name": "Paris (Toutes gares intramuros)"
              },
            ]
          },
          {
            "label": "secondList",
            "results": [
              {
                "country": "FR",
                "uicCode": "77011184",
                "name": "Gare d'Austerlitz (Paris)"
              },
              {
                "country": "FR",
                "uicCode": "77011184",
                "name": "Gare de Bercy (Paris)"
              }
            ]
          }
        ];
        // And pass it to the callback
        callback(results);
      }
    };
  }

  angular
    .module('autocomplete')
    .factory('AutocompleteService', AutocompleteService);

})();
```

#### Call an AngularJS function
###### Directive call
```
<div ng-controller="AnotherController as daddyCtrl">
  <input type="text"
    ...
    service-call="daddyCtrl.getList"
    ...
  >
</div>
```
###### Function definition
```
// In AnotherController
$scope.getList = function (search, callback) {
  // Your service goes here
  // You have to create an object that complies with this mock structure
  var results = [
    {
      "label": "firstList",
      "results": [
        {
          "country": "FR",
          "uicCode": "95027146",
          "name": "Paris (Toutes gares intramuros)"
        },
      ]
    }
  ];
  // And pass it to the callback
  callback(results);
};
```

Use
---
#### Attributes
```
<form ng-app="autocomplete|your own module">
  ...
  <autocomplete
    name="'string with simple quotes'|reference to an angular property"
    ng-model="reference to an angular property"
    service-call="reference to an angular function"
    placeholder="'string with simple quotes'|reference to an angular property"
    required="boolean"
    separated-list="boolean"
    lang="'string with simple quotes'"
    station-finder="boolean"
  >
  </autocomplete>
  ...
</form>
```
#### Default values
>**name** is the only mandatory field.

```
name: no default value
ng-model: no default value
service-call: no default value
placeholder: ""
required: false
separated-list: false
lang: 'en'
station-finder: true
```

Example
------
**index.html** contains several samples with different attribute values and code.
> **Access station finder**
> Run index.html on a host (even virtual) containing <i>voyages-sncf.com</i> to be able to get station finder.