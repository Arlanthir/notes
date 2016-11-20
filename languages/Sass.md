# Sass

## Basics

- Sass is a CSS Preprocessor that adds features like:
  - Variables
  - Imports (partials)
  - Mixins (shortcut declaration to a lot of text, with parameters)
  - Inheritance
  - Nested syntax
- Extension: .scss (.sass for alternate syntax)

### Installation

Ruby:
```
gem install sass
sass input.scss output.css
```

Node.js:
```
npm install node-sass
node-sass input.scss output.css
```

Also usable as a [gulp plugin](https://github.com/dlmanning/gulp-sass), for instance.


## Variables

```sass
$red: #ff0000;

a {
    color: $red;
}
```

## Imports

```sass
@import "header";
```

Would import file header.scss or the partial _header.scss  
Partials are not compiled to CSS on their own, they are included in the primary file(s).

## Mixins

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

## Inheritance

```sass
.message {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333;
}

.success {
    @extend .message;
    border-color: green;
}
```

## Nested Syntax

```sass
.actionable {
    &.button {
        background-color: gray;
    }
    &:hover {
        color: red;
    }

    p {    /* Means a p inside .actionable */
        font-weight: bold;
    }
}
```

## Operations

```sass
background-size: (200px/2) round(199px/2);
```

## Color Operations

```sass
rgba(#000000, 0.5);
```

## If clauses

```sass
@if ($w != 0 and $h != 0) {
} @else {
}
```

