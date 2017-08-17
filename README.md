# Website Optimization


I have optimized a provided website with a number of optimizations so that it achieves a target PageSpeed score and runs at 60 frames per second.

## Launching

The site is launced by simply opening the index.html in your preferred web browser which will lead to the optomised portfolio. Clicking on Cam's Pizzeria will lead to the pizza site.

## Portfolio Optomisations

### Minification

The Index.html file was minified to reduce the size of the response from the server.

### Font Removal

The font provided was taking a while to load so it was removed.

### Inline CSS and Javascript

The content of style.css and perfmatters.js was inserted into Index.html in order to shorten the critical rendering path

### Media Query

The print.css was given the `media="print"` attribute so that we only needed to request it from the server if we needed to print it.

### Async Javascipt

analytics.js was fetched using the async tag so that it did not block the rendering of the page.

### Image compression

The images used are now compressed, reducing their load times.


## Pizza Optomisations

### will-change

The `will-change : transform` was used to tell the browser that the pizzas in the background would need their own layer because they will be changed. This reduced the paint operations of the browser to be offloaded to the GPU through composition.

### getElementsByClassName

`document.getElementsByClassName` was used instead of document.querySelectorAll() because it is much faster.

### Don't repeat yourself

In cases where the same function call was repeatedly made, it was more efficient to store it in a variale. Take the following for example where randPizza is now used as the placeholder. This spares us the need to make the same `document.getElementsByClassName` call repeatedly.

```var randPizzas = document.getElementsByClassName("randomPizzaContainer");
    var newWidths = [];
    for (var i = 0; i < randPizzas.length; i++) {
      var dx = determineDx(randPizzas[i], size);
      newWidths.push((randPizzas[i].offsetWidth + dx) + 'px');
    }
    for (var i = 0; i < randPizzas.length; i++) {
        randPizzas[i].style.width = newWidths[i];
    }
```


### Initializing less pizzas

Previously 200 pizzas were initialized despite they all wouldn't ever be seen. This was drastically reduced which reduced Composite Layers process time.

### Avoiding Layout change

Avoiding uneccesary layout change was done as shown below. `document.body.scrollTop` was called only once. The read on `basicLeft` and edit on `left` were also seperated to avoid layout change on the DOM elements.

```var toUpdate = [];
  var bodyScrollTop = document.body.scrollTop;
  for (var i = 0; i < items.length; i++) {
    var phase = Math.sin((bodyScrollTop / 1250) + (i % 5));
    toUpdate.push(items[i].basicLeft + 100 * phase + 'px');
  }

  for (var i = 0; i < items.length; i++) {
    items[i].style.left = toUpdate[i];
  }
```
