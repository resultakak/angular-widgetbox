# Angular Widgetbox

Angular Widgetbox is module for angularJS that lets you place widgets into boxes and rearrange them by dragging them and dropping them in other boxes. No dependencies except for angularJS and document.querySelectorAll. If you've ever used iGoogle or my.yahoo - you know what I'm talking about.

The module defines 2 angular directives, `widgetbox-column` and `widgetbox-widget` which let you build widgets and boxes to place the widgets in.

## Usage

You can install angular-widgetbox with bower, or you can just checkout this repository:

`$ bower install angular-widgetbox`

### HTML

This is how your HTML codes should look like. We define boxes (columns) and inside them we define widgets. The columns need to have the `widgetbox-column` directive and the widgets need to have the `widgetbox-widget` directive.
```
<main ng-controller="MainController">
  <div class="wb-column" ng-repeat="c in columns" widgetbox-column='wb-column' widgetbox-column-id="{{ $index }}">
    <div ng-repeat="widget in c" class="wb-widget" widgetbox-widget='wb-widget' widgetbox-draghandle=".widget-header *"
    widgetbox-widget-id="{{ $index }}">
      <div class="widget-header">Drag Here</div>
      <div class="widget-contents"><!-- content here --></div>
    </div>
  </div>
</main>
```