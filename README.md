# Image-Compression-Analysis

A highly common usage of principal component analysis regarding images is image compression. A digital image is composed of a matrix of pixels, each of which is represented by its three RGB color values. PCA can reduce the dimensions of this matrix, resulting in an image that retains its most significant features while requiring significantly less storage. 

Implementation
In my implementation of PCA image compression, I applied PCA to the matrix of red, green, and blue values separately, and rejoined those matrices at the end to create the compressed image.
1.	im = np.asarray(Image.open("rowan.png")) # open the image  
2.	  
3.	r = im[:,:,0]   # matrix of red values
4.	g = im[:,:,1]   # matrix of green values
5.	b = im[:,:,2]   # matrix of blue values
6.	new_r = PCA(r)  #apply PCA to each color 
7.	new_g = PCA(g)  
8.	new_b = PCA(b)  
9.	  
10.	new_im_arr = np.dstack((new_r, new_g, new_b)) #reform the RGB matrix  
11.	new_im = Image.fromarray(new_im_arr)  
12.	new_im.save("compressed_img.png")  
My implementation of the PCA function is very similar to the previous, so I will refrain from giving another detailed explanation; additionally, there are many comments in the code for clarity. The main difference comes with the addition of the ‘num_c’ argument, representing the number of components to which the image should be reduced.
1.	def PCA(x, num_c = 3):   
2.	    x_mean = np.mean(x.T, axis = 1).T  # mean of columns
3.	    x_std = np.std(x.T, axis = 1).T   # std_dev of columns
4.	      
5.	    x = ((x - x_mean) / x_std).T  # shift and standardize data
6.	      
7.	    e_values, e_vectors = np.linalg.eigh(np.dot(x, x.T)) 
8.	    pc_size = np.size(e_vectors, axis = 1)  # find number of principal components
9.	      
10.	    idx = np.argsort(e_values)  
11.	    idx = idx[::-1]  
12.	    e_vectors = e_vectors[:,idx]  
13.	    e_values = e_values[idx]  
14.	      
15.	    if num_c < pc_size and num_c >= 0:  
16.	        e_vectors = e_vectors[:, range(num_c)] # cut some pc’s if there are too many  
17.	          
18.	    tmp = np.dot(e_vectors.T, x)  
19.	    new_im = (np.dot(e_vectors, tmp).T * x_std) + x_mean   
20.	    new_im = np.uint8(np.absolute(new_im)) # must be 'uint8' to read array as image  
21.	    return new_im  


Now, when using this 791 KB image of Rowan, the image can be compressed to any number of components between 1 and the number of pixel columns of the image—600 in this case. The results of this compression are as follows:





Analysis
All of these images are 600x600 pixels, as is the original. Miraculously, Rowan’s figure may still be discerned using only three components, and it requires only ~38% of the storage space of the original. Practical use of PCA image compression, however, may demand that no noise or artifacts should remain after compression. We can examine some higher-component compressed images. 

	
	It should be apparent that there is a large drop-off in the effects of a single component as the total number of components rises. To the human eye, these images are identical—at least at this size. The storage size of the image follows the same property; the more components there are, the less the size is affected.








	
We may also examine the effects of shifting the data so that it is mean-centered and dividing a column of pixels by its standard deviation. 



	
The similarities amongst each row imply that manipulating the data in this way does not greatly affect PCA image compression. Another factor we can check is our technique for compressing the image itself; a low-rank approximation of the singular-value decomposition (SVD) of the image is another way to accomplish similar results. 
Implementing the SVD compression is quite simple:
1.	def SVD(x, r = 20):  
2.	    u, s, v = np.linalg.svd(x)  
3.	    r = min(r, len(s))  
4.	    new_im = np.matrix(u[:, :r]) * np.diag(s[:r]) * np.matrix(v[:r, :])  
5.	    return new_im 
Using Numpy’s SVD method, we simply return the values given by the SVD up to the rank we have chosen. This yields the following results:





In terms of the image quality, the results compared to the PCA compression are quite similar: between ranks 3 & 10, the subject is discernable but the image is still heavily distorted; between 25 & 100, the subject and background are fairly clear, but there is still distortion and noise; from 300 and on, the image is hardly different from the original. In terms of storage size, too, the results are very comparable.

 
