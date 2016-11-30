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
