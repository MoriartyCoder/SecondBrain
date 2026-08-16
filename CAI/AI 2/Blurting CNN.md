
CNNs are a combination of different methods. There are three parts:
First the convolution. It is a matrix that gets moved over the image with the so called stride (step size). They work as edge detectors. We predefine the so called kernel (the matrix) with the edge we want to detect. With the stride we move over the image step by step, multiply in each step all pixels of the kernel with the pixel of the image and add the results. The result (feature map) has the shape of (𝐼 − 𝑘 + 1) × (𝑚 − 𝑙 + 1) (𝐼: pixels in the x-direction of the image, k: pixels of the kernel in the x direction, m: pixels in the y direction, l: pixels in the y direction of the kernel). Depending on the full process of the applied filter additional steps can be necessary. For example of the gabor filter, the result gets divided by the number of components of the kernel. This results in a value between the smallest possible and biggest possible value.
Means with a 3 * 3 kernel, the result of the convolution gets divided by 9.

An technique to maintain the the dimension of the input in the output is zero-padding. This adds an additional step before the convolution. We add an frame/column/row of zeros to the copy of the original image. The nessecary 

Second, there is max pooling. 

