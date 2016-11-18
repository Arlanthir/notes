# CSS - Cascading Style Sheets

## Basics
- Applies styling to webpages

## Minimal Working Example

Style file inclusion:

```html
<head>
    <link rel="stylesheet" type="text/css" href="css/style.css">
</head>
```

Styling in head section:

```html
<head>
    <style>
        p {
            ...
        }
        ...
    </style>
</head>
```

Inline styling in tags:

```html
<p style="color: #000000;"></p>
```

Style file:

```css
p {
    /* Style the paragraph tag*/
    color: #000000;
}
```

## Selectors

```css
p                    /* Tag */
#id                 /* Id */
.class             /* Class */
p a                 /* Anchor inside Paragraph */
p > a              /* Anchor direct child from Paragraph */
p + a              /* Anchor immediately after Paragraph */
p ~ a              /* Anchor after Paragraph (sibling) */
a:not(.class)   /* Anchor without class */
a:link, a:visited, a:hover, a:active    /* LoVe HAte, proper link css order */
button:hover   /* When mouse is over the element */
button:focus    /* When button is focused (after click or tab) */
#hello:target   /* Applies when the URL is <site>#hello */
input[name="log"]    /* Query including attribute */
```

## Properties

```css
border
margin
padding
width / height
text-transform: uppercase;

/* Disable selection highlight */
user-select: none;

/* Disable android blue tint on click */
-webkit-tap-highlight-color: rgba(0, 0, 0, 0);
```

## Resetting/Normalizing initial style across different browsers
http://necolas.github.io/normalize.css/

## Make a container aware of its floating children sizes
http://nicolasgallagher.com/micro-clearfix-hack/


## Make float take remaning width
```css
float: none;
overflow: hidden;
```

## Media Queries for responsive websites
```css
@media (min-width: 700px) and (max-width: 799px) {
    /* css rules for these devices */
}
```

## Responsive font-size with IE8 fallback
```css
html { font-size: 10px; }
body { font-size: 16px; font-size: 1.6rem; }
h1 { font-size: 20px; font-size: 2.0rem; }
```

## Dealing with Internet Explorer

```html
<!--[if IE 7]> <html class="ie7"> <![endif]-->
<!--[if IE 8]> <html class="ie8"> <![endif]-->
<!--[if gt IE 8]> <html> <![endif]-->
<!--[if !IE]><!--> <html> <!--<![endif]-->
```

Then you can style .ie7 and .ie8 descendants like so:

```css
.ie7 img, .ie8 img {
    border: none;
}
```

## IE 7/8 Opacity
```css
/* Order matters */
-ms-filter:"progid:DXImageTransform.Microsoft.Alpha(Opacity=50)";
filter: alpha(opacity=50);
```

## IE 7/8 Background size cover
```css
-ms-filter: "progid:DXImageTransform.Microsoft.AlphaImageLoader(src='myBackground.jpg', sizingMethod='scale')";
filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(src='.myBackground.jpg', sizingMethod='scale');
```

Or try https://github.com/louisremi/background-size-polyfill

## Avoid flickering or other rendering issues when transitioning

```css
.gpu {
     -webkit-transform: translateZ(0);
             transform: translateZ(0);
     -webkit-perspective: 1000;
             perspective: 1000;
     -webkit-transform-style: preserve-3d;
             transform-style: preserve-3d;
}
```

## Simulate background-size: cover and contain with `<img>`

```css
img.img-cover {
    /* Center the image */
    position: absolute;
    top: -99999px;
    bottom: -99999px;
    left: -99999px;
    right: -99999px;
    margin: auto;
    /* Simulate background-size: cover. Note: it will only scale up */
    min-width: 100%;
    min-height: 100%;
     /* Simulate background-size: contain. Note: it will only scale down */
    max-width: 100%;
    max-height: 100%;
}
```

## Style input type="file" without Javascript
Use the `<label>` element!

http://stackoverflow.com/questions/572768/styling-an-input-type-file-button

http://jsfiddle.net/4cwpLvae/

## Good Google Fonts
- Lato
- Open Sans
