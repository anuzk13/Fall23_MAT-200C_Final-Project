
- Intro
“While the continuous methods helps optimization, the stochastic sampling required for rendering is costly and can result in noise.” ([Kerbl et al., 2023, p. 1](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=1&annotation=EPBIXZB3))
1. “We first introduce 3D Gaussians as a flexible and expressive scene representation. We start with the same input as previous NeRF-like methods, i.e., cameras calibrated with Structure-from-Motion (SfM)” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=KRHU4GDL))
	- “anisotropic 3D Gaussians as a high-quality, unstructured representation of radiance fields.” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=I4FWWWU5)) [[NERF Paper#^469223]]
		- An anisotropic object or pattern has properties that differ according to direction of measurement. 
1. “The second component of our method is optimization of the properties of the 3D Gaussians – 3D position, opacity 𝛼, anisotropic covariance, and spherical harmonic (SH) coefficients – interleaved with adaptive density control steps, where we add and occasionally remove 3D Gaussians during optimization” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=N7S8KRX6))“. 
2. The third and final element of our method is our real-time rendering solution that uses fast GPU sorting algorithms and is inspired by tile-based rasterization, following recent work [Lassner and Zollhofer 2021]. However, thanks to our 3D Gaussian representation, we can perform anisotropic splatting that respects visibility ordering – thanks to sorting and 𝛼blending – and enable a fast and accurate backward pass by tracking the traversal of as many sorted splats as required” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=F48KVICK))

- Related work
Survey Paper[ Neural Fields in Visual Computing and Beyond](zotero://select/library/items/XU9BNQG2) and [website](https://neuralfields.cs.brown.edu/) 
- a field is not the same as the algebraic concept...
- The adoption of the term "field" in computer science, particularly in neural fields like NeRF, is more recent and is metaphorically borrowed from physics. Here, the term denotes a continuous mapping of values across a space, akin to how a physical field maps quantities like temperature or magnetic intensity across a region. 
- [Lumigraph](file:///C:/Users/Ana/Downloads/download.pdf)
- Traditional scene reconstruction and rendering
	- [Light field](https://en.wikipedia.org/wiki/Light_field) rendering
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
	- Structure-from-Motion (SfM)
		- https://www.youtube.com/playlist?list=PL2zRqk16wsdoYzrWStffqBAoUY8XdvatV
			- https://www.youtube.com/watch?v=oIvg7sbJRIA&list=PL2zRqk16wsdoYzrWStffqBAoUY8XdvatV&index=7
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
					- TBC: https://www.youtube.com/watch?v=0BVZDyRrYtQ&list=PL2zRqk16wsdoYzrWStffqBAoUY8XdvatV&index=11
	- multi-view stereo (MVS)
		- “All these methods re-project and blend the input images into the novel view camera, and use the geometry to guide this re-projection.” ([Kerbl et al., 2023, p. 2](zotero://select/library/items/YUP8JCNQ)) ([pdf](zotero://open-pdf/library/items/DA2P2IAG?page=2&annotation=CPVV8GLJ))
- Neural Rendering and Radiance Fields:
	- Cool history of Nerf: https://neuralradiancefields.io/history-of-neural-radiance-fields/
		- [Photosculptures](https://en.wikipedia.org/wiki/Photo_sculpture)!!
		- !