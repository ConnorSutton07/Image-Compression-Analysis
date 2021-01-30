# Image-Compression-Analysis

A highly common usage of principal component analysis regarding images is image compression. A digital image is composed of a matrix of pixels, each of which is represented by its three RGB color values. PCA can reduce the dimensions of this matrix, resulting in an image that retains its most significant features while requiring significantly less storage. 

## PCA Results
 
![image](https://user-images.githubusercontent.com/55513603/106340387-3ffea400-625f-11eb-93b4-e36370399fa1.png)

All of these images are 600x600 pixels, as is the original. Miraculously, Rowan’s figure may still be discerned using only three components, and it requires only ~38% of the storage space of the original. Practical use of PCA image compression, however, may demand that no noise or artifacts should remain after compression. We can examine some higher-component compressed images. 

![image](https://user-images.githubusercontent.com/55513603/106340400-4db42980-625f-11eb-83f4-83dd9b56c272.png)

It should be apparent that there is a large drop-off in the effects of a single component as the total number of components rises. To the human eye, these images are identical—at least at this size. The storage size of the image follows the same property; the more components there are, the less the size is affected.

![image](https://user-images.githubusercontent.com/55513603/106340415-599feb80-625f-11eb-8ece-7fcc903acc7d.png)



## SVD Results

![image](https://user-images.githubusercontent.com/55513603/106340436-67557100-625f-11eb-8a34-20184e33191a.png)

In terms of the image quality, the results compared to the PCA compression are quite similar: between ranks 3 & 10, the subject is discernable but the image is still heavily distorted; between 25 & 100, the subject and background are fairly clear, but there is still distortion and noise; from 300 and on, the image is hardly different from the original. In terms of storage size, too, the results are very comparable.

![image](https://user-images.githubusercontent.com/55513603/106340456-73d9c980-625f-11eb-91d0-31d258e131b5.png)
