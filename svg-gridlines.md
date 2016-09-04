Svg gridlines
=================
2016-09-04



If you want to experiment with a css grid model,
then having some background lines showing where the columns are might be useful.


The code below uses svg to draw a canvas of 12 vertical lines.

The bonus you get with svg is that the lines stretch/shrink automatically as the window is resized.

Also, it's easy to change the number of lines and the appearance too.

To change the number of lines n, update the value v of the pattern's width attribute.
The formulae is:

```
v = 1 / n
So for instance for 12 lines:
v = 1 / 12 ~= 0.08333
```


There are many ways to use the svg grid.
A simple way s to put the svg grid as the background image of an element (like the body element for instance).



Here is the html:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title>Html page</title>
    <style>
        html, body {
            height: 100%;
        }

        body {
            padding: 0px;
            margin: 0px;
            background: url(svg-grid.svg) repeat top left;
            width: 100%;
            height: 100%;
        }
    </style>
</head>

<body>

</body>
</html>
```


And here is the svg:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
    <defs>
        <pattern id="pat" x="0" y="0" width=".08333" height="1">
            <rect class="gridline" x="0" y="0" width="1" height="100%" fill="black" opacity=".1"></rect>
        </pattern>
    </defs>
    <rect x="0" y="0" width="100%" height="100%" fill="url(#pat)"></rect>
</svg>
```


