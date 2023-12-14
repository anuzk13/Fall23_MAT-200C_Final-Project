
# Abstract

“For unbounded and complete scenes (rather than isolated objects)” ([Kerbl et al., 2023, p. 1](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=1&annotation=3C633DHX)) -> most Nerf examples are centered on an object 
“allow high-quality real-time (≥ 30 fps) novel-view synthesis at 1080p resolution” ([Kerbl et al., 2023, p. 1](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=1&annotation=YEN4Q4DK))
# Intro
“While the continuous methods helps optimization, the stochastic sampling required for rendering is costly and can result in noise.” ([Kerbl et al., 2023, p. 1](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=1&annotation=EPBIXZB3))
1. “We first introduce 3D Gaussians as a flexible and expressive scene representation. We start with the same input as previous NeRF-like methods, i.e., cameras calibrated with Structure-from-Motion (SfM)” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=KRHU4GDL))
	- “anisotropic 3D Gaussians as a high-quality, unstructured representation of radiance fields.” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=I4FWWWU5)) [[NERF Paper#^469223]]
		- An anisotropic object or pattern has properties that differ according to direction of measurement. 
1. “The second component of our method is optimization of the properties of the 3D Gaussians – 3D position, opacity 𝛼, anisotropic covariance, and spherical harmonic (SH) coefficients – interleaved with adaptive density control steps, where we add and occasionally remove 3D Gaussians during optimization” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=N7S8KRX6))“. 
2. The third and final element of our method is our real-time rendering solution that uses fast GPU sorting algorithms and is inspired by tile-based rasterization, following recent work [Lassner and Zollhofer 2021]. However, thanks to our 3D Gaussian representation, we can perform anisotropic splatting that respects visibility ordering – thanks to sorting and 𝛼blending – and enable a fast and accurate backward pass by tracking the traversal of as many sorted splats as required” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=F48KVICK))

# Related work
Survey Paper[ Neural Fields in Visual Computing and Beyond](zotero://select/library/items/XU9BNQG2) and [website](https://neuralfields.cs.brown.edu/) 
- a field is not the same as the algebraic concept...
- The adoption of the term "field" in computer science, particularly in neural fields like NeRF, is more recent and is metaphorically borrowed from physics. Here, the term denotes a continuous mapping of values across a space, akin to how a physical field maps quantities like temperature or magnetic intensity across a region. 
- [Lumigraph](file:///C:/Users/Ana/Downloads/download.pdf)
## Traditional scene reconstruction and rendering
### [Light field](https://en.wikipedia.org/wiki/Light_field) rendering
- https://graphics.stanford.edu/projects/lightfield/
- Marc Levoy https://www.youtube.com/watch?v=THzykL_BLLI
	- Light field 5D Plenoptic Function
		- ![[Pasted image 20231130171636.png|275]]
		- Can be reduced to 4 dimensions by assuming no occlusions
	- Good explanation of the two plane parametrization: http://people.csail.mit.edu/billf/www/papers/lightfieldsTR-Levin-Freeman-Durand-08.pdf
		- https://www.youtube.com/watch?v=q_zVV89nU3g&list=PLuRaSnb3n4kSgSV35vTPDRBH81YgnF3Dd&index=35&t=601s
	- Synthetic aperture
		- If an object is smaller than the lens (i,e a needle in front of the eye) then it does not occlude the lens
		- You can simulate a lens focusing on a point by using an array of cameras and averaging the pixel corresponding to that point
		- Using an array of cameras you [can see trough stuff](https://youtu.be/THzykL_BLLI?si=yUQhR-9IOAbmQw_S&t=682). Using a synthetic aperture (array of cameras) you can see trough larger objects 
	- Using camera filter to create a light field which allows
		- re-focusing
		- Digitally moving the observer with macrophotography : https://youtu.be/THzykL_BLLI?si=ZjJDSUvnwxO5fkwz&t=1113
- Paul Debevec https://youtu.be/lRK0MtIyj0U?si=rcXJwY84t7KLNG3B&t=3083
	- ![[Pasted image 20231130162005.png]]
	- You can render scenes inside the sphere by taking the pixels from different cameras
### Structure-from-Motion (SfM)
- https://www.youtube.com/playlist?list=PL2zRqk16wsdoYzrWStffqBAoUY8XdvatV
	- casual or uncontrolled video: Motion of the camera during capture is not known
	- from the video you can get 3d structure of the scene and the camera movement
	- SFM Observation Matrix 
		- low rank matrix
		- structure matrix and motion matrix
		- Tomasi-Kenade Factorization method
		- ![[Pasted image 20231204135536.png|400]]
		- ![[Pasted image 20231204140332.png|400]]
			- Very special form of the observation matrix - low rank 
				- ![[Pasted image 20231204141622.png|450]]
			- Using Singular Valor Decomposition the matrix can be de-composed, because the rank is is less than 3 we know that sigma has at most 3 non-zero diagonal elements
			- ![[Pasted image 20231206141453.png|375]]
			- This way we can come up with a compressed form of the matrices 
				- ![[Pasted image 20231206142320.png|500]]\
			- To find the M and S matrices we can use a unique representation of the decomposition that leaves a matrix incognito Q
				- I dont understand why finding Q leads to a unique valid factorization
			- ![[Pasted image 20231206143516.png|475]]
			- Then using some properties of the camera motion matrix M (Orthonormality) to solve for Q

				- ![[Pasted image 20231206143407.png|350]]![[Pasted image 20231206143247.png|350]]
			- The result is a matrix M with the camera frames and the matrix S which are the 3D points that were projected for each of those camera frames. These points contain brightness and color as well
				- ![[Pasted image 20231206144226.png|325]]
### multi-view stereo (MVS)
- See also alex reich lecture on tobias class about MVS networks [[CS291-I Visual Computing and Interaction –Extended Reality (XR)#3D reconstruction]]
- “All these methods re-project and blend the input images into the novel view camera, and use the geometry to guide this re-projection.” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=CPVV8GLJ))
- “a large part of the recent success of MVS is due to the success of the underlying Structure from Motion (SfM) algorithms that compute the camera parameters.” ([Furukawa and Hernández, 2015, p. 15](zotero://select/library/items/XKJZZ4K3)) ([pdf](zotero://open-pdf/library/items/EHX5LHCA?page=15&annotation=9LMEDQEY))
	- SFM “systems were mainly designed for structured sets of images i.e., sets where the order of images matters” ([Furukawa and Hernández, 2015, p. 21](zotero://select/library/items/XKJZZ4K3)) ([pdf](zotero://open-pdf/library/items/EHX5LHCA?page=21&annotation=XR8AU5LL))
	- “recent MVS applications also use unordered sets of images captured at different times with different hardware, e.g. 3D maps from aerial images” ([Furukawa and Hernández, 2015, p. 22](zotero://select/library/items/XKJZZ4K3)) ([pdf](zotero://open-pdf/library/items/EHX5LHCA?page=22&annotation=5HRKL5MP))
	- “The development of fast and high quality feature detectors [87, 135, 57] and descriptors [135, 36, 159, 130, 26] was a crucial development towards making SfM work with unstructured datasets. High quality descriptors enabled building longer and higher quality tracks from images captured with very different pose and illumination.” ([Furukawa and Hernández, 2015, p. 22](zotero://select/library/items/XKJZZ4K3)) ([pdf](zotero://open-pdf/library/items/EHX5LHCA?page=22&annotation=3JM6EMXE))
- “MVS shares the same principles with such classic stereo algorithms, MVS algorithms are designed to deal with images with more varying viewpoints, such as an image set surrounding an object, and also deal with a very large number of images, even in the order of millions.” ([Furukawa and Hernández, 2015, p. 24](zotero://select/library/items/XKJZZ4K3)) ([pdf](zotero://open-pdf/library/items/EHX5LHCA?page=24&annotation=W4YNUIDI))
- ![[Pasted image 20231206155038.png|475]] ([pdf](zotero://open-pdf/library/items/EHX5LHCA?page=25&annotation=63XNFXQL))  ([Furukawa and Hernández, 2015, p. 25](zotero://select/library/items/XKJZZ4K3))
- ![[Pasted image 20231206155930.png|300]] ([pdf](zotero://open-pdf/library/items/EHX5LHCA?page=50&annotation=KE2YX9PP))  ([Furukawa and Hernández, 2015, p. 50](zotero://select/library/items/XKJZZ4K3))
- ![[Pasted image 20231206160002.png|300]] ([pdf](zotero://open-pdf/library/items/EHX5LHCA?page=52&annotation=F6CGTD5F))  ([Furukawa and Hernández, 2015, p. 52](zotero://select/library/items/XKJZZ4K3))
## Neural Rendering and Radiance Fields:
- Cool history of Nerf: https://neuralradiancefields.io/history-of-neural-radiance-fields/
	- [Photosculptures](https://en.wikipedia.org/wiki/Photo_sculpture)!!
- Combining MVS meshes or depth maps was not an efficient use of neural networks
- Volumetric rendering
	- “Volumetric representations for novel-view synthesis were initiated by Soft3D [Penner and Zhang 2017]” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=KHJEBBHZ))
		- “we show that by designing a system end-to-end for view synthesis (rather than using an existing outputs such as a 3D meshes or point clouds), we can produce improved synthesis results that challenge existing methods for a wide variety of inputs (such as views from plenoptic cameras, camera array videos, and wide baseline image captures).” ([Penner and Zhang, 2017, p. 2](zotero://select/library/items/G2TSAE7J)) ([pdf](zotero://open-pdf/library/items/4VIBAJCD?page=2&annotation=Q79NATWQ))
		- “Image based rendering (IBR) approaches typically use proxy geometry to synthesize views, while stereo and multi-view stereo (MVS) algorithms reconstruct geometry from images.” ([Penner and Zhang, 2017, p. 2](zotero://select/library/items/G2TSAE7J)) ([pdf](zotero://open-pdf/library/items/4VIBAJCD?page=2&annotation=NZMHFEC4))
		- **[Light-field rendering is part of IBR](https://graphics.stanford.edu/courses/cs248-08/IBR-and-phot-san.pdf)** 
		- This is a nice survey paper of IBR techniques, where would Gaussian Splats go?
		- ![[Pasted image 20231207112824.png|400]] ([pdf](zotero://open-pdf/library/items/BQ6MRULV?page=5&annotation=MNTSPY8R))  ([Kang et al., 2006, p. 176](zotero://select/library/items/II68L4GR))
	- “deep-learning techniques coupled with volumetric ray-marching were subsequently proposed https://www.computationalimaging.org/publications/deepvoxels-learning-persistent-3d-feature-embeddings/
		- It was slow to do ray marching here
	- “Neural Radiance Fields (NeRFs) [Mildenhall et al. 2020] introduced importance sampling and positional encoding to improve quality, but used a large Multi-Layer Perceptron negatively affecting speed.” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=NDRW8D6X))
		- [[NERF Paper]]
	- Newer methods use “sparse voxel grid to interpolate a continuous density field” ([Kerbl et al., 2023, p. 3](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=3&annotation=V2NDI8ZV))  and “rely on Spherical Harmonics: the former to represent directional effects directly, the latter to encode its inputs to the color network” ([Kerbl et al., 2023, p. 3](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=3&annotation=BZ7BG9ND))
		- sparse voxel grid
			- [Instant NGP](https://github.com/NVlabs/instant-ngp)
			- [Plenoxels](https://alexyu.net/plenoxels/)
		- Spherical harmonics are a sequence of specially defined functions that are often used in physical and mathematical problems involving functions on the surface of a sphere. 
			- a function on a sphere is one that takes a point on the sphere (usually defined by two angles, such as latitude and longitude) and returns a value. For example, a function might return the temperature at each point on Earth's surface.
			-  https://basesandframes.wordpress.com/2016/05/11/spherical-harmonic-lighting-the-gritty-details/
			- https://www.gdcvault.com/play/1022720/Spherical-Harmonic-Lighting-The-Gritty
			- https://basesandframes.wordpress.com/2016/05/11/spherical-harmonic-lighting-the-gritty-details/
			- https://www.gdcvault.com/play/334/Stupid-Spherical-Harmonics-(SH)
## Point based rendering and Radiance Fields

- “point sample rendering [Grossman and Dally 1998] rasterizes an unstructured set of points with a fixed size, for which it may exploit natively supported point types of graphics APIs” ([Kerbl et al., 2023, p. 3](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=3&annotation=U8RBES8N))
	- [Points as a display primitive](https://graphics.stanford.edu/papers/points/)
- Splats: “Seminal work on high-quality point-based rendering addresses these issues by “splatting” point primitives with an extent larger than a pixel,” ([Kerbl et al., 2023, p. 3](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=3&annotation=MM3EUF38)) circular or elliptic discs, ellipsoids, or surfel
	- “This paper describes a point rendering and texture filtering technique called surface splatting which directly renders opaque and transparent surfaces from point clouds without connectivity. It is based on a novel screen space formulation of the Elliptical Weighted Average filter.” ([Zwicker et al., 2001, p. 1](zotero://select/library/items/HDNUC4QI)) ([pdf](zotero://open-pdf/library/items/3N4XR9Z2?page=1&annotation=ZRQBAUNT))
	- [PointShop 3D](https://cgl.ethz.ch/pointshop3d/)
	- Cool demo of rendering of surface splats on web: 
		- https://www.willusher.io/webgl-ewa-splatter/#Dinosaur
		- https://github.com/Twinklebear/webgl-ewa-splatter?tab=readme-ov-file
- “Point-based 𝛼-blending and NeRF-style volumetric rendering share essentially the same image formation model. Specifically, the color 𝐶 is given by volumetric rendering along a ray:” ([Kerbl et al., 2023, p. 3](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=3&annotation=HYWCLVMS))
	- ![[Pasted image 20231210171854.png|275]]
	- “From Eq. 2 and Eq. 3, we can clearly see that the image formation model is the same. However, the rendering algorithm is very different.” ([Kerbl et al., 2023, p. 3](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=3&annotation=NGWLIC7I))
		- NERFs has an implicit representation that has to be sampled
		- Points are an explicit representation 
	- Other representations include Spheres
		- “Differentiable rendering is the foundation for modern neural rendering approaches, since it enables end-to-end training of 3D scene representations from image observations. However, gradient-based optimization of neural mesh, voxel, or function representations suffers from multiple challenges, i.e., topological inconsistencies, high memory footprints, or slow rendering speeds. To alleviate these problems, Pulsar employs: 1) a sphere-based scene representation, 2) an efficient differentiable rendering engine, and 3) neural shading.” ([Lassner and Zollhöfer, 2020, p. 1](zotero://select/library/items/ZZIRP2P7)) ([pdf](zotero://open-pdf/library/items/XJCZUPT3?page=1&annotation=KB5F6WNK))
- Other methods have use 3D gaussians or 3D primitives
	- [Neural 3d primitives](https://stephenlombardi.github.io/projects/mvp/)
- For their algorithm:
	- explicit representations like points don't have problem with empty space
	- “we want to maintain (approximate) conventional 𝛼-blending on sorted splats to have the advantages of volumetric representations:” ([Kerbl et al., 2023, p. 3](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=3&annotation=GQFTK4PZ)) ... like NERFs they want to keep track of the ray order of the points?

#  Method Overview
- Input
	- “a set of images of a static scene,
	- corresponding cameras calibrated by SfM [Schönberger and Frahm 2016] which produces a sparse point cloud as a side effect.” ([Kerbl et al., 2023, p. 4](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=4&annotation=JMWKGCKG))
- 1. create “3D Gaussians (Sec. 4), defined by:
	- a position (mean), 
	- covariance matrix 
	 - opacity 𝛼” ([Kerbl et al., 2023, p. 4](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=4&annotation=8HUCCHLV))
 - 2. create a radiance field “via a sequence of optimization steps of 3D Gaussian parameters, i.e., position, covariance, 𝛼 and SH coefficients interleaved with operations for adaptive control of the Gaussian density.” ([Kerbl et al., 2023, p. 4](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=4&annotation=52XZVQ4D))
	 - “The directional appearance component (color) of the radiance field is represented via spherical harmonics (SH), following standard practice” ([Kerbl et al., 2023, p. 4](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=4&annotation=7HCX3LWP))
		 - [Spherical harmonics](https://nvlabs.github.io/instant-ngp/)
 - 3. rasterization “our tile-based rasterizer (Sec. 6) that allows 𝛼-blending of anisotropic splats, respecting visibility order thanks to fast sorting.” ([Kerbl et al., 2023, p. 4](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=4&annotation=XJVYT2GZ))

![[Pasted image 20231210183525.png]] ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=5&annotation=RHJXUM3A))  ([Kerbl et al., 2023, p. 5](zotero://select/library/items/YUP8JCNQ))
# Differentiable 3D Gausian Splatting

The differential rasterization is on [this repository](https://github.com/graphdeco-inria/diff-gaussian-rasterization)

- Requirements of a primitive:
	- “we need a primitive that inherits the properties of differentiable volumetric representations, (like SDF) while at the same time being unstructured (like points) and explicit to allow very fast rendering.” ([Kerbl et al., 2023, p. 4](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=4&annotation=3Y3GRWMP))
- Gaussians:
	$G(x) = e^{-\frac{1}{2} x^T \Sigma^{-1} x}$
	$\Sigma$ -> Covariance Matrix, defines how the Gaussian blob is shaped in 3D space. It determines how wide or narrow the Gaussian is in each direction (x, y, z). It also defines the orientation of the Gaussian distribution by describing the relationship between the variables. For example, if the Gaussian is elongated in one direction, the covariance matrix will show a larger value for that direction.
	$x^T \Sigma^{-1} x$ -> This is a matrix multiplication and results in a scalar value. The expression essentially measures how far the point $x$ is from the center of the Gaussian distribution, taking into account the shape and orientation of the distribution as described by $\Sigma$. The farther away  $x$ is, the larger this value becomes. 
	$e^{-\frac{1}{2} x^T \Sigma^{-1} x}$ -> This exponent determines the "height" of the Gaussian function at the point $x$. Points closer to the center of the Gaussian will have a higher value (as the exponent approaches zero), while points further away will rapidly decrease towards zero, creating the bell shape.
<iframe src="https://editor.p5js.org/anuzk13/full/viVJ6-H7L"  name="myiFrame" scrolling="no" frameborder="1" marginheight="0px" marginwidth="0px" height="600px" width="100%" allowfullscreen></iframe>


<iframe src="https://colab.research.google.com/drive/1S7y61iD-t7_esXekjIiDEt6eSTJiPaDf?usp=sharing" style="border:0px #ffffff none;" name="myiFrame" scrolling="no" frameborder="1" marginheight="0px" marginwidth="0px" height="600px" width="100%" allowfullscreen></iframe>

- Rendering optimization
- Projecting gaussians in 2D
	- $\Sigma' = \mathbf{J} \mathbf{W} \boldsymbol{\Sigma} \mathbf{W}^T \mathbf{J}^T$
		- $\mathbf{J}$ Jacobian -> 
	- The specific part that implements this projection is the [computeCov2D function](https://github.com/graphdeco-inria/diff-gaussian-rasterization/blob/59f5f77e3ddbac3ed9db93ec2cfe99ed6c5d121d/cuda_rasterizer/forward.cu#L74C19-L74C31)
	- the Jacobian matrix of a transformation contains all the first-order partial derivatives of a vector-valued function. It describes how small changes in the input (in this case, the 3D coordinates of a point) lead to changes in the output (the transformed 2D coordinates). 
	- ![[Pasted image 20231212133937.png|325]] ([pdf](zotero://open-pdf/library/items/YIMU4QNB?page=9&annotation=FX74HRT4))  ([“EWA splatting | IEEE Journals & Magazine | IEEE Xplore”, p. 9](zotero://select/library/items/4FNQQ8LJ))
- <iframe src="https://colab.research.google.com/drive/1T9aEOS9QtggameDbr32P_GK6i4lP3Zc3?usp=sharing" style="border:0px #ffffff none;" name="myiFrame" scrolling="no" frameborder="1" marginheight="0px" marginwidth="0px" height="600px" width="100%" allowfullscreen></iframe>

# Optimization with adaptive density control of 3D gaussians

