## To Call directly less in HTML
```
<link rel="stylesheet/less" href="/stylesheets/main.less" type="text/css" />
<script src="http://lesscss.googlecode.com/files/less-1.0.30.min.js"></script>
```

## Node Installation
```
npm install -g less
```

## Using REPL
``` 
lessc theme.less > theme.css
```

## Using package.json
```
"scripts": {
  "less:compile": "lessc theme.less > theme.css"
}
npm run less:compile
```

## 1.  Cleaner Structure With Nesting

  # LESS Usage
  ```
  #header {}
  #header #nav {}
  #header #nav ul {}
  #header #nav ul li {}
  #header #nav ul li a {}
  ```
  > LESS Usage
  ```
  #header {
    #nav {
      ul {
        li {
          a {}
        }
      }
    }
  }
  ```
## 1.1.  Bubbling:

  .component {
    width: 300px;
    @media (min-width: 768px) {
      width: 600px;
      @media  (min-resolution: 192dpi) {
        background-image: url(/img/retina2x.png);
      }
    }
    @media (min-width: 1280px) {
      width: 800px;
    }
  }

## 2.  Variables For Faster Maintenance

  # Normal CSS
  #header, #footer, h3 { color: red; }

  # LESS Usage
  @brand-color: #4455EE;
  #header { background-color: @brand-color; }
  #footer { color: @brand-color; }
  h3 { color: @brand-color; }

## 2.1.  Variable Interpolation:

# Variables
@my-selector: banner;
@images: "../img";

# Usage
.@{my-selector} {
  font-weight: bold;
  background: url("@{images}/white-sand.png");
}

## 2.2.  Scoping:

  @great-color: #4455EE;
  #header {
    @great-color: #EE3322;
    color: @great-color;
  }

  # Output

  #header { color: #EE3322; }

## 3. Reusing Whole Classes / Mixin

  .rounded-corners {
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    border-radius: 5px;
  }

  #login-box {
    .rounded-corners;
  }

## 3.1. With mixin with arguments:

  .rounded-corners(@radius: 5px) {
    -webkit-border-radius: @radius;
    -moz-border-radius: @radius;
    border-radius: @radius;
  }

  #login-box {
    .rounded-corners(10px);
  }

## 3.2 If you don't want the mixin to appear as a rule in the CSS

  #circle(){
    background-color: #4CAF50;
    border-radius: 100%;
  }

  #big-circle{
    width: 100px;
    height: 100px;
    #circle
  }

## 4.  Operations

  @base-margin: 25px;
  @color: #03A9F4;

  #header { margin-top: @base-margin + 10px; }
  div { background-color: @color - 100; }

## 5.  Namespaces And Accessors

  #defaults {
    @heading-color: #EE3322;
    .bordered { border: solid 1px #EEE; }
  }

  # Calling

  h1 {
    color: #defaults[@heading-color];
    #defaults > .bordered;
  }

## 5.1. Maps:

  .sidebar_heading { color: h1['color']; }

  #colors() {
    primary: blue;
    secondary: green;
  }

  .button {
    color: #colors[primary];
    border: 1px solid #colors[secondary];
  }

## 6.  Functions

  @var1: #004590;
  @var2: 50vh/2;
  div{
    height: 50px;
    width: calc(50% + (@var2 - 20px));  // result is calc(50% + (25vh - 20px))
    background-color: @var1;

    &:hover{
      background-color: fadeout(@var1, 50%)
    }
  }

## 6.1. More functions:

  @base: #f04615;
  @width: 0.5;

  .class {
    width: percentage(@width); // returns 50%
    color: saturate(@base, 5%);
    background-color: spin(lighten(@base, 25%), 8);
  }

  @selectors: blue, green, red;
  each(@selectors, {
    .sel-@{value} {
      a: b;
    }
  });

  @set: {
    one: blue;
    two: green;
    three: red;
  }
  .set {
    each(@set, {
      @{key}-@{index}: @value;
    });
  }

## 7. Escaping

@min768: ~"(min-width: 768px)";

(or)

@min768: (min-width: 768px);

.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}

## 8. Importing

@import 'typography';
@import 'layout';

## 9. Themes using Guard Mixins

# Example 1
@color_info: #00a8e6;
@color_success: #8cc14c;
@color_warning: #faa732;
@color_error: #da314b;

.alert(@mode) when (@mode = 'info'){
  background-color: @color_info;
  border: thin solid darken(@color_info, 15%);
}

.alert(@mode) when (@mode = 'success'){
  background-color: @color_success;
  border: thin solid darken(@color_success, 15%);
}

.alert(@mode) when (@mode = 'warning'){
  background-color: @color_warning;
  border: thin solid darken(@color_warning, 15%);
}

.alert(@mode) when (@mode = 'error'){
  background-color: @color_error;
  border: thin solid darken(@color_error, 15%);
}

.alert(@mode) {
  width: 300px;
  padding: 25px;
  color: #fff;
  text-shadow: 0.5px 0.5px #000;
}

# Example 2
  @orange-color: #f26524;
  @grey-color: #333333;
  @white-color: #ffffff;
  @set: {
      orange: orange;
      grey: grey;
      white: white;
  }
  .checkwhite (@mode) when (@mode = 'white') {
      border: 1px solid @grey-color;
  }
  each(@set, {
    @my-color: "@{value}-color";
    .@{value}-bg-clr {
        background: @@my-color;
        .checkwhite("@{value}");
    }
  });
