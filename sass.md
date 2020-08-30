## Node Installation
npm install node-sass

## Using SASS 
sass --watch scss/index.scss:public/css/style.css

## Using REPL 
node-sass input.scss output.css

## Using package.json
```
"scripts": {
  "sass:compile": "node-sass --include-path sass INPUT_FILE.scss OUTPUT_FILE.css",
}
npm run sass:compile
```
## 1.  Variables
```
  $title-font: normal 24px/1.5 'Open Sans', sans-serif;
  $cool-red: #F44336;
  $box-shadow-bottom-only: 0 2px 1px 0 rgba(0, 0, 0, 0.2);

  h1.title {
    font: $title-font;
    color: $cool-red;
    box-shadow: $box-shadow-bottom-only;
  }
```
> to
```
  h1.title {
    font: normal 24px/1.5 "Open Sans", sans-serif;
    color: #F44336;
    box-shadow: 0 2px 1px 0 rgba(0, 0, 0, 0.2);
  }
```
## 1.1 Scopes and Global
```
  $primaryColor: #eeccff;

  body {
    $primaryColor: #ccc !global;
    background: $primaryColor;
  }
  p {
    color: $primaryColor;
  }
```
> to
```
  body {
    background: #ccc;
  }

  p {
    color: #ccc;
  }
```
## 2.  Mixins
```
  @mixin square($size, $color) {
    width: $size;
    height: $size;
    background-color: $color;
  }
  .small-blue-square {
    @include square(20px, rgb(0,0,255));
  }

  @mixin transform-tilt() {
    $tilt: rotate(15deg);
    -webkit-transform: $tilt; /* Ch <36, Saf 5.1+, iOS, An =<4.4.4 */
        -ms-transform: $tilt; /* IE 9 */
            transform: $tilt; /* IE 10, Fx 16+, Op 12.1+ */
  }
  .frame:hover { 
    @include transform-tilt; 
  }

  @mixin media($queryString){
      @media #{$queryString} {
        @content;
      }
  }
  .container {
      width: 900px;
      @include media("(max-width: 767px)"){
          width: 100%;
      }
  }
```
## 3.  Extend
```
  .dialog-button {
    box-shadow: 0 1px 1px 0 rgba(0, 0, 0, 0.12);
  }
  .confirm {
    @extend .dialog-button;
    background-color: #87bae1;
  }
```
  to
```
  .dialog-button, .confirm {
    box-sizing: border-box;
    box-shadow: 0 1px 1px 0 rgba(0, 0, 0, 0.12);
  }
  .confirm {
    background-color: #87bae1;
  }
```
## 4.  Nesting
```
  ul {
    list-style: none;
    li {
      padding: 15px;
    }
  }
```
> to
```
  ul {
    list-style: none; 
  }

  ul li {
    padding: 15px;
  }
```
## 5.  Operations
```
$container-width: 100%;
$column-count: 4;
$margin: 1%;

.container { 
  width: $container-width;
  height: $container-width / 2;
}
.column {
  width: ($container-width / $column-count) - ($margin * 2);
  margin: 0 $margin;
}
```
## 6.  Functions
```
  $awesome-blue: #2196F3;

  a {
    padding: 10px 15px;
    background-color: $awesome-blue;
    &:hover {
      background-color: darken($awesome-blue,10%);
    }
  }

  @function getColumnWidth($width, $columns, $margin){
      @return ($width / $columns) - ($margin * 2);
  }

  $container-width: 100%;
  $column-count: 4;
  $margin: 1%;

  .column {
    width: getColumnWidth($container-width,$column-count,$margin);
    margin: 0 $margin;
  }
```
## 7. Theme Development
> Example 1
```
$settings: (
    maxWidth: 800px,
    columns: 12,
    margin: 15px,
    breakpoints: (
        xs: "(max-width : 480px)",
        sm: "(max-width : 768px) and (min-width: 481px)",
        md: "(max-width : 1024px)  and (min-width: 769px)",
        lg: "(min-width : 1025px)"
    )   
);

@mixin renderGridStyles($settings){
  .container {
    padding-right: map-get($settings, "margin");
    padding-left: map-get($settings, "margin");
    margin-right: auto;
    margin-left: auto;
    max-width: map-get($settings,"maxWidth");
  }
  
  .row {
    margin-right: map-get($settings, "margin") * -1;
    margin-left: map-get($settings, "margin") * -1;
  }
  $breakpoints: map-get($settings, "breakpoints");
  @each $key, $breakpoint in $breakpoints {
    @include media($breakpoint) {
      @include renderGrid($key, $settings);
    }
  }
}

@mixin renderGrid($key, $settings) {
  $i: 1;
  @while $i <= map-get($settings, "columns") {
    .col-#{$key}-#{$i} {
      float: left;
      width: 100% * $i / map-get($settings,"columns");
    }
    $i: $i+1;
  }
}

@mixin media($queryString){
    @media #{$queryString} {
      @content;
    }
}

@include renderGridStyles($settings);
```
> Example 2
```
@mixin themable($theme-name, $container-bg, $left-bg, $right-bg, $innertext, $button-bg) {
   .#{$theme-name} {
       .wrapper {
           background-color: $container-bg;
           border: 1px solid #000;
           display: flex;
           height: 500px;
           justify-content: space-between;
           margin: 0 auto;
           padding: 1em;
           width: 50%;

           * {
               color: $innertext;
               font-size: 2rem;
           }

           .left {
               background-color: $left-bg;
               height: 100%;
               width: 69%;
           }

           .right {
               background-color: $right-bg;
               height: 100%;
               position: relative;
               width: 29%;
           }

           .button {
               background-color: $button-bg;
               border: 0;
               border-radius: 10px;
               bottom: 10px;
               cursor: pointer;
               font-size: 1rem;
               font-weight: bold;
               padding: 1em 2em;
               position: absolute;
               right: 10px;
           }
       }
   }
}

@include themable(theme-1, #f7eb80, #497265, #82aa91, #fff, #bc6a49);
@include themable(theme-2, #e4ada7, #d88880, #cc6359, #fff, #481b16);
```
