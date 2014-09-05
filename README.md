# Angular Widgetbox

Angular Widgetbox is module for angularJS that lets you place widgets into boxes and rearrange them by dragging them and dropping them in other boxes. No dependencies except for angularJS and document.querySelectorAll. If you've ever used iGoogle or my.yahoo - you know what I'm talking about.

The module defines 2 angular directives, `widgetbox-column` and `widgetbox-widget` which let you build widgets and boxes to place the widgets in.

## Usage

You can install angular-widgetbox with bower, or you can just checkout this repository:

`$ bower install angular-widgetbox`

### HTML

Here's an example HTML:

```html
<main ng-controller="MainController">
  <div class="wb-column" ng-repeat="c in columns" widgetbox-column='wb-column' widgetbox-column-id="{{ $index }}">
    <div ng-repeat="widget in c" class="wb-widget" widgetbox-widget='wb-widget' widgetbox-draghandle=".widget-header"
    widgetbox-widget-id="{{ $index }}">
      <div class="widget-header">Drag Here</div>
      <div class="widget-contents"><!-- content here --></div>
    </div>
  </div>
</main>
```

Let's go over the code:

- We are using the `ng-repeat` directives to make multiple columns and place multiple widgets inside them. This is already covered by the angularjs documentation.
- The `widgetbox-column` directive is placed on the container boxes that will act as receptacles for the widgets. The value of the directive **must** be the css class name assigned to each of the container boxes. In our case, it's "wb-column"
- The `widgetbox-column-id` directive needs to contain the following value: `{{ $index }}`. We need to somehow reference the position of the column, and the loop index is perfect for that.
- The `widgetbox-widget` directive is placed on the actual widgets that we want to move around. Again, its value is the classname assigned to all the widgets. In our case - "wb-widget"
- The `widgetbox-draghandle` directive needs to be a CSS selector that points to an element inside each widget. This element will be initialized as a handle by which you can drag the widget. If the directive is omitted, then the entire widget will act as a handle.
- The `widgetbox-column-id` directive needs to contain the following value: `{{ $index }}`. We need to somehow reference the position of the widget inside the column, and the loop index is perfect for that.

### CSS

You can style the boxes and widgets however you like. Here's a simple stylesheet to get you started:

```css
/**
 * We have 4 columns, and we want them one next to the other.
**/
.wb-column {
  float: left;
  width: 25%;
  min-height: 100%;
}
.wb-widget {
  box-sizing: border-box;
  margin: 0 5px 15px;
  border: solid 1px #ccc;
}
/**
 * This is the little box that appears in the place where the currently dragged widget 
 * will be placed if you dropped it immediately.
**/
.widgetbox-placeholder {
  height: 100px;
  margin: 0 0 10px;
  background: rgba(0,0,0,0.25);
  margin: 0 5px 10px;
}
```

### Javascript

The javascript part is very simple. You just need to define a `widgetboxOnWidgetMove` function and attach it to the controller's scope. This function will be called each time a widget is dragged into a different position.

```javascript
var app = angular.module('AngularWidgetboxApp', ['widgetbox']);

app.controller('MainController', function($scope){
  // We have 4 columns with one widget in each of them
  $scope.columns = [ [{ }], [{ }], [{ }], [{ }] ];

  // The angular-widgetbox directive calls this method when a widget is moved
  $scope.widgetboxOnWidgetMove = function(sourceColumn, sourcePosition, targetColumn, targetPosition){
    $scope.$apply(function(){
      if(targetColumn){
        var widget = $scope.columns[parseInt(sourceColumn, 10)].splice(parseInt(sourcePosition, 10), 1)[0];
        var target = $scope.columns[parseInt(targetColumn, 10)];
        targetPosition == 'last' ? target.push(widget) : target.splice(parseInt(targetPosition, 10), 0, widget);
      }
    });
  }
});
```

The `widgetboxOnWidgetMove` function will be passed 4 arguments:

- `sourceColumn` - Integer, the index of the column where the widget used to be.
- `sourcePosition` - Integer, the index of the widget inside the column.
- `targetColumn` - Integer, the index of the column where the widget is dropped.
- `targetPosition` - Mixed, If it's an integer, it's the index where the widget is dropped. Otherwise if it's the string "last", the widget is being dropped at the bottom of the column.

## License

MIT


