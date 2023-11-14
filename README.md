For my project I  am interested in reading a paper on NeRF and implementing this methods on webgpu. I am interested in being able to read, understand and implement a ML paper. I will use the same license as the original NERF project (MIT License).

My question would be:

Do you recommend me to try to understand the paper and the math? I have seen some "cool things" in the internet and I am not sure if instead I should just try to play around with it and achieve a good looking result.

### Previous work on **NeRF** [Representing Scenes as Neural Radiance Fields for View Synthesis](https://www.matthewtancik.com/nerf)

*My notes on the [NERF Paper](NERF%20Paper.md)*
**Conclusion**
	- Nerfs build on volume rendering techniques and combine them with the idea of representing 3d scene as a continuous function (to reduce size) and do some estimations of the volume rendering techniques to make it a differentiable function, and use some other optimizations such as training a coarse and detailed network to optimize ray sampling and also re-encoding the parameters of the network (camera position and orientation) because they found out that when the rendering function maps to high frequency domains of color and shape the network does not perform well...so they do some re-mapping that I don't 100% understand to make it a lower frequency function
	- To render them on web I need to [bake them](https://phog.github.io/snerg/)
	- It is yet unclear to me what is the relation between Nerfs and Gaussian Splatting but the second seems to be the one available and explored in web versions https://github.com/antimatter15/splat 
	- https://poly.cam/tools/gaussian-splatting 
	- https://twitter.com/antimatter15?lang=en
## Possible explorations

### 3D Gaussian Splatting for Real-Time Radiance Field Rendering
[gaussian splatting](https://github.com/graphdeco-inria/gaussian-splatting) 
![595](ezgif-3-85421be3e1.gif)


Modifying [GP with shaders](https://x.com/Ruben_Fro/status/1712651517310996701?s=20)

![ezgif-3-8c7bc2777f](ezgif-3-8c7bc2777f.gif)

### Sources for volumetric video
[Creating Gaussian Splats from movies](https://youtu.be/8CdLVVny9hc?feature=shared)

### Dynamic volumetric video
[Dynamic Gaussian Splatting](https://dynamic3dgaussians.github.io/?s=09)

![600](ezgif-3-616785f8ff.gif)
- [Enerf](https://zju3dv.github.io/enerf/)
- [Kplanes](https://sarafridov.github.io/K-Planes/)
- [4K4D: Real-Time 4D View Synthesis at 4K Resolution](https://zju3dv.github.io/4k4d/?s=09) (only paper available, no code...yet?)

### Web implementations
[Web implementations](https://whenistheweekend.com/k/GaussianSplats3D/demo/fanPhone.html?s=09)
[GP for threejs](https://github.com/mkkellogg/GaussianSplats3D?s=09)


https://x.com/AlexR4/status/1720378611289526471?t=eaLqUYpj3C02ZcTXi852Xw&s=09

https://x.com/amygoodchild/status/1724083526671163645?t=8kaS-UfnGKgxmOJ3fQk1hw&s=09