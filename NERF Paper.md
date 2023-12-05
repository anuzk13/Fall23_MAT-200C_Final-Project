- https://colab.research.google.com/github/bmild/nerf/blob/master/tiny_nerf.ipynb
- [video ](https://www.youtube.com/watch?v=dPWLybp4LL0)

**Conclusion**
	- Nerfs build on volume rendering techniques and combine them with the idea of representing 3d scene as a continuous function (to reduce size) and do some estimations of the volume rendering techniques to make it a differentiable function, and use some other optimizations such as training a coarse and detailed network to optimize ray sampling and also re-encoding the parameters of the network (camera position and orientation) because they found out that when the rendering function maps to high frequency domains of color and shape the network does not perform well...so they do some re-mapping that I don't 100% understand to make it a lower frequency function
	- To render them on web I need to [bake them](https://phog.github.io/snerg/)
	- It is yet unclear to me what is the relation between Nerfs and Gaussian Splatting but the second seems to be the one available and explored in web versions https://github.com/antimatter15/splat 
	- https://poly.cam/tools/gaussian-splatting 
	- https://twitter.com/antimatter15?lang=en

**Reading notes**
- Intro
	- “Our method optimizes a deep fully-connected neural network without any convolutional layers (often referred to as a multilayer perceptron or MLP)” ([Mildenhall et al., 2020, p. 1](zotero://select/library/items/8FBKWBMU)) ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=1&annotation=SA4L2B4Q))
	- “We represent a static scene as a continuous 5D function that 
		- outputs the **radiance field (radiance emitted in each direction (θ, φ) at each point (x, y, z) in space)** ^469223
		- and a density at each point which acts like a differential opacity controlling how much radiance is accumulated by a ray passing through (x, y, z)” ([Mildenhall et al., 2020, p. 1](zotero://select/library/items/8FBKWBMU)) ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=1&annotation=N4CY546K))
	- “To render this neural radiance field (NeRF)”: 
		- “1) march camera rays through the scene to generate a sampled set of 3D points, 
		- 2) use those points and their corresponding 2D viewing directions as input to the neural network to produce an output set of colors and densities, and 
		- 3) use classical volume rendering techniques to accumulate those colors and densities into a 2D image. 
	- Because this process is naturally differentiable, we can use gradient descent to optimize this model by minimizing the error between each observed image and the corresponding views rendered from our representation. 
	- Minimizing this error across multiple views encourages the network to predict a coherent model of the scene by assigning high volume densities and accurate colors to the locations that contain the true underlying scene content.” ([Mildenhall et al., 2020, p. 2](zotero://select/library/items/8FBKWBMU)) ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=2&annotation=RFFL2G3G))
	- Optimization
		- “transforming input 5D coordinates with a positional encoding that enables the MLP to represent higher frequency functions, and we propose a hierarchical sampling procedure to reduce the number of queries required to adequately sample this high-frequency scene representation.” ([Mildenhall et al., 2020, p. 2](zotero://select/library/items/8FBKWBMU)) ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=2&annotation=J4SNX7J3))

- Related work
	- Neural 3D shape representations
		- Continuous functions that represent a 3d scene:
			- ![Pasted image 20231113214512](Pasted%20image%2020231113214512.png)
		- “A promising recent direction in computer vision is encoding objects and scenes in the weights of an MLP that directly maps from a 3D spatial location to an implicit representation of the shape, such as the signed distance” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=3&annotation=NYIJC6LX))
			- Implicit representations (SDF)
				- https://youtu.be/B2BTSKcYqtQ?si=Vr70sa2Ss47uKfJH&t=155
				- https://alpha.womp.com/projects/442905
				- https://editor.p5js.org/triplezero/sketches/IeE6fvLY7
			- “implicit representation of continuous 3D shapes as level sets by optimizing deep networks that map xyz coordinates to signed distance functions” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=3&annotation=U6DEKZUW)) -> requires 3D models
				- http://www.maxjiang.ml/proj/lig
			- These wo
			- Niemeyer, M., Mescheder, L., Oechsle, M., Geiger, A.: Differentiable volumetric rendering: Learning implicit 3D representations without 3D supervision. In: CVPR (2019)
				- https://github.com/autonomousvision/differentiable_volumetric_rendering
			- Sitzmann, V., Zollhoefer, M., Wetzstein, G.: Scene representation networks: Continuous 3D-structure-aware neural scene representations. In: NeurIPS (2019)
				- https://github.com/vsitzmann/scene-representation-networks
	- View synthesis and image-based rendering
		- “Given a dense sampling of views, photorealistic novel views can be reconstructed by simple light field sample interpolation techniques [21,5,7].” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=4&annotation=RGDW8IJX))
			- [Light field rendering ](http://www.cs.cmu.edu/afs/cs/academic/class/16823-s16/www/T2P4.pdf)
		- “For novel view synthesis with sparser view sampling, the computer vision and graphics communities have made significant progress by predicting traditional geometry and appearance representations from observed images” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=4&annotation=XWVCYM84))
		- mesh-based methods.
			- Differentiable rasterizers or pathtracer can directly optimize mesh representations to reproduce a set of input images using gradient descent. this strategy requires a template mesh with fixed topology to be provided
			- [Kaolin library](https://github.com/NVIDIAGameWorks/kaolin)
			- [ Learning to Predict 3D Objects with an Interpolation-based Differentiable Renderer](https://research.nvidia.com/labs/toronto-ai/DIB-R/)
			- [Differentiable Monte Carlo Ray Tracing through Edge Sampling](https://people.csail.mit.edu/tzumao/diffrt/)
		- volumetric representations -> discrete volumetric representations like voxel grids or planes
			- ![Pasted image 20231113213804](Pasted%20image%2020231113213804.png)
			- “used large datasets of multiple scenes to train deep networks that predict a sampled volumetric representation from a set of input images, and then use either alpha-compositing or learned compositing along rays to render novel views at test time.” ([Mildenhall et al., 2020, p. 4](zotero://select/library/items/8FBKWBMU)) ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=4&annotation=DDFDBT7D)). “their ability to scale to higher resolution imagery is fundamentally limited by poor time and space complexity due to their discrete sampling” ([Mildenhall et al., 2020, p. 4](zotero://select/library/items/8FBKWBMU)) ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=4&annotation=E3KVBLA7))
			- [DeepView View Synthesis with Learned Gradient Descent](https://augmentedperception.github.io/deepview/)
		
- NERF
	“We represent a continuous scene as a 5D vector-valued function whose input is a 3D location x = (x, y, z) and 2D viewing direction (θ, φ), and whose output is an emitted color c = (r, g, b) and volume density σ.” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=4&annotation=CETDTPAE))
	
	- “We encourage the representation to be multiview consistent by restricting the network to predict the volume density σ as a function of only the location x, while allowing the RGB color c to be predicted as a function of both location and viewing direction.” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=5&annotation=DNZMZAYE))
		- “the MLP FΘ first processes the input 3D coordinate x with 8 fully-connected layers... and outputs σ and a 256-dimensional feature vector. This feature vector is then concatenated with the camera ray’s viewing direction and passed to one additional layer... that output the view-dependent RGB color.”  ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=5&annotation=3V7EPA8Y))
	
- Volume rendering with radiance fields
	- ![Pasted image 20231113215201](Pasted%20image%2020231113215201.png)
	- 5D radiance fields (3D volumes with 2D view-dependent appearance)
	- A volumetric version of [Surface Light Fields](https://grail.cs.washington.edu/projects/slf/)
	- “render the color of any ray passing through the scene using principles from classical[ volume rendering](https://www.youtube.com/watch?v=y4KdxaMC69w)  ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=5&annotation=RWSHXLPR))
	![Pasted image 20231113205713](Pasted%20image%2020231113205713.png)
	- “We numerically estimate this continuous integral using quadrature. Deterministic quadrature, which is typically used for rendering discretized voxel grids, would effectively limit our representation’s resolution because the MLP would only be queried at a fixed discrete set of locations. Instead, we use a stratified sampling approach where we partition [tn, tf ] into N evenly-spaced bins” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=6&annotation=7AIFY6V5))

- Optimizing a nerf
	- Positional encoding
		- “directly operate on xyzθφ input coordinates results in renderings that perform poorly at representing high-frequency variation in color and geometry.” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=7&annotation=V3PTPQAT))
		- deep networks are biased towards learning lower frequency functions.
		- “positional encoding.”([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=8&annotation=FL2QIMB9))
			- ![Pasted image 20231113212602](Pasted%20image%2020231113212602.png)
		- FΘ as a composition of two functions FΘ = F ′ Θ ◦ γ,
	- Hirarchical volume sampling
		- “hierarchical representation that increases rendering efficiency by allocating samples proportionally to their expected effect on the final rendering.” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=8&annotation=K6YI759Q))
		- “we simultaneously optimize two networks: one “coarse” and one “fine”. We first sample a set of Nc locations using stratified sampling, and evaluate the “coarse” network at these locations. Given the output of this “coarse” network, we then produce a more informed sampling of points along each ray where samples are biased towards the relevant parts of the volume.” ([pdf](zotero://open-pdf/library/items/JMSMQL9S?page=8&annotation=7ZTLKKKI))
		- ![Pasted image 20231113213137](Pasted%20image%2020231113213137.png)
		
- Implementation
	- [Colmap](https://github.com/colmap/colmap) for camera poses and intrinsic parameters
	- Minimize the rendering error of the network with respect to the training images
		- ![250](Pasted%20image%2020231113215918.png)
	- 