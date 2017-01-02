# HTML

## Basics

HyperText Markup Language - Define webpages content
- .html files - code

## Minimal Working Example
```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>My Webpage</title>
<head>

<body>
</body>

</html>
```

## Basic tags

```html
<h1>Header</h1>
<p>Paragraph</p>
<a href="otherpage.html">Link</a>
<img src="image.png" alt="description">
<div>Structural Block</div>
<span>Inline Element</span>

<table>
    <tr>
        <td>Table Cell in Table Row in Table</td>
    </tr>
</table>
```

## Some HTML5 Semantic Tags
```html
<header>
<footer>
<hgroup> <!-- Groups h1 to h6 headings -->
<article>
<section>
<nav> <!-- Navigation icons -->
<progress> <!-- Progress of a task -->
```

## Make mobile devices use device width instead of emulating a desktop computer
```html
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
</head>
```

## Escaped characters
```
& &amp;
space &nbsp;
< &lt;
> &gt;
ç &ccedil;
Á &Aacute;
à &agrave;
ã &atilde;
â &acirc;
```

## Facebook OpenGraph info
```html
<head>
    <meta property="og:type" content="article"/>
    <meta property="og:title" content="My Article"/>
    <meta property="og:site_name" content="My Website"/>
    <meta property="og:image" content="http://www.site.com/squareImage.png"/>
    <meta property="og:description" content="Yada yada yada"/>
    <meta property="og:url" content="http://www.site.com/canonical_url.php"/>
</head>
```

## Search Engine Optimization
```html
<head>
    <link rel="canonical" href="http://www.site.com/product.php?id=23" /> <!-- Prevent duplicate content from id=23&otherparam=true -->
    <title>Thing | Category | Brand</title>
    <meta name="description" content="A new thingie relating to the Category!">
</head>

<img src="..." alt="Image description">
```

Use only one &lt;h1&gt; tag per page

## Favicon
```html
<head>
    <link rel="shortcut icon" href="favicon.ico">
</head>
```

More icons are needed for iphone, Android, etc... Check online tools

## Newsletters

### Guidelines
- Inline styles for each element
- Layout through `<table>`s
- Explicit width in `<td>`s
- Explicit width and height in images, both as attributes and CSS
- Display block in images
- Images must use explicit height/width, both in CSS as width/height html4 attributes
- Images must have the `alt` attribute. If none is wanted, use `alt=""`
- Use margins of `<div>` children instead of paddings in adjacent `<td>`s

```html
<body style="margin: 0; border: 0; padding: 0; background-color: #c9e4f7;">
    <center>
        <table style="margin: 0 auto; border: 0; padding: 0; width: 900px; border-spacing: 0; text-align: left; table-layout: fixed;" cellspacing="0">
            <colgroup>
                <col style="width: 87px;">
                <col style="width: 606px;">
                <col style="width: 118px;">
                <col style="width: 89px;">
            </colgroup>

            <!-- Fix for Outlook: because we use colspan in the first line -->
            <tr style="margin: 0; border: 0; padding: 0;">
                <td width="87" style="margin: 0; border: 0; padding: 0;"></td>
                <td width="606" style="margin: 0; border: 0; padding: 0;"></td>
                <td width="118" style="margin: 0; border: 0; padding: 0;"></td>
                <td width="89" style="margin: 0; border: 0; padding: 0;"></td>
            </tr>
        </table>
    </center>
</body>
```

### Show images without proxy in GMail

Run in the Javascript console

```javascript
(function(){ while(img = document.evaluate('//img[contains(@src, \'googleusercontent.com\')]', document,null,XPathResult.FIRST_ORDERED_NODE_TYPE,null).singleNodeValue){ var src = img.attributes.src.value; src = src.substr(src.indexOf('#')+1); img.attributes.src.value = src; } })();
```
