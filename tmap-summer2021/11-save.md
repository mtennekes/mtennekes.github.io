# Save maps {#save}

Maps created programmatically can serve several purposes, from exploratory, through visualizations of the processing steps, to being the final outputs of a given project.
Therefore, often we want just to see our map on the screen, but sometimes we also want to save our map results to an external file.
**tmap** objects can be directly saved to output files with `tmap_save()`^[Standard R approach of saving graphic files by opening a graphic device, e.g., `png()`, plotting the data, and then closing the device with `dev.off()` also works.].

The `tmap_save()` function allows to save our map in three groups of file formats, (a) raster graphics (section \@ref(raster-graphic-formats)), (b) vector graphics (section \@ref(vector-graphic-formats)), and (c) interactive ones (section \@ref(interactive-format)).
Additionally, chapter \@ref(animations) shows how to save map animations with the use of the `tmap_animation()` function.

For the examples in this section, we will use a simple map of the Easter Island polygon with the island name superimposed (not shown), stored in the `tm` object.


```r
library(tmap)
library(sf)
ei_borders = read_sf("data/easter_island/ei_border.gpkg")
tm = tm_shape(ei_borders) +
  tm_polygons() +
  tm_text("name", fontface = "italic")
tm
```

## Raster graphic formats

Raster graphics are non-spatial relatives of spatial rasters.
The digital images are composed of many pixels - squares filled with specific colors.
Main raster graphic file formats include PNG, JPEG, BMP, and TIFF.
<!-- jn: should we describe each in one sentence? -->
<!-- explain DPI -->

Saving **tmap** objects to a file can be done with the `tmap_save()`.
It usually accepts two arguments^[In fact, one argument is enough - if you just provide a **tmap** object, then it will be saved to a `tmap01` file with some default format.] - the first one, `tm`, is our map object, and the second one, `filename`, is the path to the created file.


```r
tmap_save(tm, "my_map.png")
#> Map saved to /home/mtes/git/tmap-book/my_map.png
#> Resolution: 2492 by 1769 pixels
#> Size: 8.31 by 5.9 inches (300 dpi)
```

By default, DPI is set to 300, and the image width and height is automatically adjusted based on the map aspect ratio.
These parameters can be, however, changed with the `dpi`, `width`, and `height` arguments^[You can even specify just one of `width` or `height`, and the value of the second one will be calculated using the formula `asp = width / height`.].


```r
tmap_save(tm, "my_map.png", width = 1000, height = 750, dpi = 300)
#> Map saved to /home/mtes/git/tmap-book/my_map.png
#> Resolution: 1000 by 750 pixels
#> Size: 3.33 by 2.5 inches (300 dpi)
```

The units of `width` or `height` depend on the value you set. 
They are pixels (`"px"`) when the value is greater than 50, and inches (`"in"`) otherwise.
Units can also be changed with the `units` argument.

This function also has several additional arguments, including `outer.margins`, `scale` and `asp`.
<!-- ref to the layout chapter -->
All of them override the arguments' values set in `tm_layout()`.
Additionally, when set to `0`, the' asp' argument has a side effect: it moves the map frame to the edges of the image.



```r
tmap_save(tm, "my_map.png", device = ragg::agg_png)
#> Map saved to /home/mtes/git/tmap-book/my_map.png
#> Resolution: 2492 by 1769 pixels
#> Size: 8.31 by 5.9 inches (300 dpi)
```



## Vector graphic formats


```r
tmap_save(tm, "my_map.svg")
#> Map saved to /home/mtes/git/tmap-book/my_map.svg
#> Size: 8.31 by 5.9 inches
```

<!-- scale -->
<!-- ref to the fonts section -->

## Interactive format


```r
tmap_save(tm, "my_map.html")
#> Interactive map saved to /home/mtes/git/tmap-book/my_map.html
```

<!-- add.titles -->
<!-- in.iframe -->
<!-- selfcontained -->
<!-- arguments passed on to device functions or to saveWidget or saveWidgetframe -->