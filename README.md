# Content based Image Retrieval
## Introduction

We wish to implement an improved unsupervised planogram compliance model. 
One of the standing challenges in building an efficient algorithm for this task is identifying vacant locations in the planogram image where the object or objects are missing. We propose a method to address the same through background detection using SURF features along with color-based features followed by K-means clustering.

## Assumptions

>In this method, we have assumed that the planogram image will cover enough field of view such that most of the pixels are corresponding to the background, that is, the non-object part.
>The planogram image will not have vertical stacking of objects (or products). 

## Method
 
Let the height of the planogram image be h pixels, and w be its width in pixels. We extract color-based features as well as SURF features and apply K-means clustering to detect the background portion of the image.


### Color-based features: 


First, we divide the original planogram image into smaller squares of size k [(k pixels) x (k pixels)]. This gives us a total of **n** smaller squares, where [ n = (h // k) * (w // k) ]. Using those n square, we apply K-means clustering on the pixel color values across the three color channels (RGB) to get a total of **n_clusters** number of clusters. Finally, we create a plot of the clustered image by using the formula [ p[i] = (255 * i) / (n_clusters - 1) ]; given, number of clusters is always greater than 1. Here, **p[i]** represents the pixel intensity value of the ith cluster and i is a natural number in the closed range [1, n_clusters - 1]. We store the final plot values in **color_map**. The clustering is done such that the larger cluster receives lesser pixel value.  

### SURF features:



Similarly, we divide the original planogram image into smaller squares of size k_des, giving us **n_des** number of squares. We extract the SURF features for each smaller square and use the descriptor values to apply K-means clustering. We retrieve a **n_cluster_des** number of clusters and create a plot of the clustered image as we did before. We store the final plot values in **color_map_des**.

### Combining the data from the two types of features:



We define a variable **alpha** belonging to the open interval of real numbers (0, 1). We get the final plot values using both **color_map** and **color_map_des**.

**color_map_sum = alpha*(color_map_des) + (1 - alpha)*color_map**

The final result is improved using opening morphological transformation.



### Graph Plot:
We plot the number of colored pixels (pixel value not equal to zero) in each column of the image [ size of column = (**h**, 1) ] against the column number [ 0 to (**w** - 1) ]. The plotted graph is smoothened using the exponential moving average.



## Conclusion

Any dip in the final plotted graph corresponds to the background region detected by this approach. The background region corresponds to the non-product region or empty spot in the planogram image.

This approach can be further improved by improving on the first assumption. This can be done by enhancing our algorithm in such a manner that the cluster with the least SURF features or most sparse distribution of SURF features corresponds to the background.



