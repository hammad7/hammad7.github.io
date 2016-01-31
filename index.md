---
title       : SVD on JPEG image
subtitle    : Dimensionalty Reduction
author      : Anonymous
job         : Data Products' Student
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---


## How this App works

This App applies Singular Vector Decomposition on a JPEG image. It reduces the number of features to the input provided by the user through slider. Since the image can now be represented from less number of features its size is reduced. Features of all 3 colors i.e RED, GREEN and BLUE are reduced separately. The image is re-constructed to original dimensions but from a reduced set of features. The variance retained for each color is shown in the left sidebar panel. The variance tab shows plots that represent the portion of image data that is retained from the original image. This is plotted for each color. The user can upload any jpg image or select from dropdown list and then can download the reconstructed image.

---


```r
img <- readJPEG('./data/Flower.jpg')
  y.r <- img[,,1]
  y.g <- img[,,2]
  y.b <- img[,,3]
  z.r <<- scale(y.r)
  z.g <<- scale(y.g)
  z.b <<- scale(y.b)
  svd1.r <<- svd(z.r)
  svd1.g <<- svd(z.g)
  svd1.b <<- svd(z.b)
  descale <- function(mat,att){
  t(t(mat)*att$'scaled:scale'+att$'scaled:center')
}
g<-5
      p.r <- descale(svd1.r$u[,1:g] %*% diag(svd1.r$d[1:g]) %*% t(svd1.r$v[,1:g]),attributes(z.r))
      p.g <- descale(svd1.g$u[,1:g] %*% diag(svd1.g$d[1:g]) %*% t(svd1.g$v[,1:g]),attributes(z.g))
      p.b <- descale(svd1.b$u[,1:g] %*% diag(svd1.b$d[1:g]) %*% t(svd1.b$v[,1:g]),attributes(z.b))
      
      myp <- array(c(p.r,p.g,p.b),dim=dim(img))
      writeJPEG(myp,'data/temp.jpg',1)
```

---

Eg:

Lets take only 5 features and try to reconstruct this image from these.


![Original Image](data/Flower.jpg)
Original Image


---

Image after Reconstruction

![Reconstruction](data/temp.jpg)