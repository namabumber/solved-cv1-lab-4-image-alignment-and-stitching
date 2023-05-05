Download Link: https://assignmentchef.com/product/solved-cv1-lab-4-image-alignment-and-stitching
<br>
<h1><a name="_Toc4584"></a>1             Image Alignment</h1>

In this practice, you will write a function that takes two images as input and computes the affine transformation between them. You will work with supplied <em>boat </em>images. The overall scheme is as follows:

<ol>

 <li>Detect interest points in each image.</li>

 <li>Characterize the local appearance of the regions around interest points.</li>

 <li>Get the set of supposed matches between region descriptors in each image.</li>

 <li>Perform RANSAC to discover the best transformation between images.</li>

</ol>

<h3>Hint</h3>

The first three steps can be performed using David Lowe’s SIFT. Check out the Docs of SIFT related function for further information in the following link: <a href="https://docs.opencv.org/master/da/df5/tutorial_py_sift_intro.html">https://docs.opencv.org/master/da/df5/tutorial_py_sift_intro.html</a> and <a href="https://docs.opencv.org/3.4.9/d5/d3c/classcv_1_1xfeatures2d_1_1SIFT.html">https://docs.opencv.org/3.4.9/d5/d3c/classcv_1_1xfeatures2d_1_1SIFT. </a><a href="https://docs.opencv.org/3.4.9/d5/d3c/classcv_1_1xfeatures2d_1_1SIFT.html">html</a>

For some patent issue, the newest version of OpenCV does not contain SIFT related function. However, you can install a old version (for example: opencv-python==3.4.2.17 and opencv-contrib-python==3.4.2.17)

The final stage, running RANSAC, should be performed as follows:

<ul>

 <li>Repeat <em>N </em>times:</li>

 <li>Pick <em>P </em>matches at random from the total set of matches <em>T</em>.</li>

 <li>Construct a matrix <em>A </em>and vector <em>b </em>using the <em>P </em>pairs of points and find transformation parameters (<em>m</em>1<em>,m</em>2<em>,m</em>3<em>,m</em>4<em>,t</em>1<em>,t</em>2) by solving the equation <em>Ax </em>= <em>b</em>.</li>

 <li>Using the transformation parameters, transform the locations of all <em>T </em>points in image1. If the transformation is correct, they should lie close to their counterparts in image2. Plot the two images side by side with a line connecting the original <em>T </em>points in image1 and transformed <em>T </em>points over image2.</li>

 <li>Count the number of inliers, where inliers are defined as the number of transformed points from image1 that lie within a radius of 10 pixels of their pair in image2.</li>

</ul>

(1)

It can be rewritten as

(2)

or

(3)

Figure 1: Affine transformation. • If this count exceeds the best total so far, save the transformation parameters and the set of inliers.

<ul>

 <li>End repeat.</li>

 <li>Finally, transform image1 using this final set of transformation parameters. If you display this image, you should find that the pose of the object in the scene should correspond to its pose in image2. To transform the image, implement your own function based on nearest-neighbor interpolation. Then use the OpenCV built-in function warpAffine and compare your results.</li>

</ul>

<h3>Hint</h3>

Nearest neighbors does not mean you have to classify points. The problem is that if you have a transformation, then the transformed points may not be at perfect pixels (e.g., 0.3px). Instead of linear interpolation, which requires more work to implement, we can just use nearest neighbors which means simply rounding the coordinates.

<h3>Question – 1 (55-<em>pts</em>)</h3>

<ol>

 <li>Create a function that takes image pairs <em>pgm </em>and <em>boat2.pgm </em>as input, and return the keypoint matchings between the two images. Name your script as <strong>keypoint matching.py</strong>.</li>

 <li>Take a random subset (with set size set to 10) of all matching points, and plot on the image. Connect matching pairs with lines. You can assign a random color to each line to make them easier to distinguish.</li>

 <li>Create a function that performs the RANSAC algorithm as explained above. The function should return the best transformation found. For visualization, show the transformations from image1 to image2 and from image2 to image1. Name your script as <strong>py</strong>.</li>

</ol>

Include a demo function to run your code.

<h3>Question – 2 (5-<em>pts</em>)</h3>

<ol>

 <li>How many matches do we need to solve an affine transformation which can be formulated as shown in Figure 1?</li>

 <li>How many iterations in average are needed to find good transformation parameters?</li>

</ol>

<h1><a name="_Toc4585"></a>2             Image Stitching</h1>

In this part, you will write a function that takes two images as input and stitch them together. The method described in the previous section will be used to stitch two images together by transforming one of them to the coordinate space of the other. You will work with supplied images <em>left.jpg </em>and <em>right.jpg</em>. The overall scheme can be summarized as follows:

<ol>

 <li>As in previous task you should first find the best transformation between input images.</li>

 <li>Then you should estimate the size of the stitched image.</li>

 <li>Finally, combine the <em>jpg </em>with the transformed <em>right.jpg </em>in one image.</li>

</ol>