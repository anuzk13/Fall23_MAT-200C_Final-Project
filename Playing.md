- https://www.youtube.com/watch?v=UXtuigy_wYc 

## Using software

- Used polycam to create a Gaussian Splat -> failed no explanation
- Luma worked with the small file

## Creating my own splats locally

Running the code on this [repo](https://github.com/graphdeco-inria/gaussian-splatting) with guidance from[ this video](https://www.youtube.com/watch?v=UXtuigy_wYc)

### Training the model
#### Failed

- Installing cuda toolkits
	- Had to only leave VS2019
	- Had to [install NsightVSE separately](https://developer.nvidia.com/gameworksdownload)
	- After installing cuda and other dependencies and [adding cl.exe to the path](https://www.reddit.com/r/Cplusplus/comments/x6nfpr/how_to_add_clexe_to_path/)  (from vs studio [here ](C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Auxiliary\Build))
		- The authors also talk about that [here](https://github.com/graphdeco-inria/gaussian-splatting#local-setup:~:text=It%20still%20doesn%27t,and%20try%20this%3B)
	- ...and also checking [some of the recommendations online](https://github.com/graphdeco-inria/gaussian-splatting/issues/297) it worked
	- [This person summarized the steps I am following](https://github.com/graphdeco-inria/gaussian-splatting/issues/332)
- I will run out of memory:
	- Found maybe a [toy model](https://www.reshot.ai/3d-gaussian-splatting)
	- This person[ has recommendations](https://www.jeromeswannack.com/projects/2023/10/11/splats.html#:~:text=Gaussian%20Splatting%20Scripts,of%20success%20(YMMV).) for not running out of memory
		The Gaussian Splatting repository contains two scripts:
		
		convert.py
		train.py
		With the project conda environment activated, from the repository root, I run this script:
		
		python convert.py -s $DATA_DIR
		python train.py -s $DATA_DIR
		If I run out of VRAM, I will adjust these parameters for train.py:
		
		Increase these values:
		--densify_grad_threshold, starts at 0.0002
		--densification_interval, starts at 100
		Decrease this value:
		--densify_until_iter, starts at 15_000
		This has worked to varying degrees of success (YMMV).
	- I think I can't run cuda yet
		- ![[Pasted image 20231116112026.png]]
		- NVIDIA GeForce RTX 3080 Laptop GPU with CUDA capability sm_86 is not compatible with the current PyTorch installation.
		- The current PyTorch install supports CUDA capabilities sm_37 sm_50 sm_60 sm_61 sm_70 sm_75 compute_37.
		- If you want to use the NVIDIA GeForce RTX 3080 Laptop GPU GPU with PyTorch, please check the instructions at https://pytorch.org/get-started/locally/
			- pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
			- pip install torch==1.9.0+cu102 torchvision==0.10.0+cu102 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html
		- I will try my own
			- First...install pytorch - https://pytorch.org/get-started/locally/ 
			- conda create -n gaussian_splatting_2 python=3.7
			- conda activate gaussian_splatting_2 
			- pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
				- doesnt work
			- conda install pytorch==1.13.1 torchvision==0.14.1 torchaudio==0.13.1 pytorch-cuda=11.7 -c pytorch -c nvidia
```
name: gaussian_splatting
channels:
  - pytorch
  - conda-forge
  - defaults
dependencies:
  - cudatoolkit=11.6
  - plyfile=0.8.1
  - python=3.7.13
  - pip=22.3.1
  - pytorch=1.12.1
  - torchaudio=0.12.1
  - torchvision=0.13.1
  - tqdm
  - pip:
    - submodules/diff-gaussian-rasterization
    - submodules/simple-knn
```


- conda install -c anaconda vs2019_win-64

- Getting some c.DLL error
	- Found [this solution](https://github.com/graphdeco-inria/gaussian-splatting/issues/306)
	- C:\Users\Ana\anaconda3\envs\gaussian_splatting_2\lib\site-packages\s
	- Installed c:\users\ana\anaconda3\envs\gaussian_splatting_2\lib\site-packages\simple_knn-0.0.0-py3.7-win-amd64.egg
- conda install pytorch==1.13.1 torchvision==0.14.1 torchaudio==0.13.1 pytorch-cuda=11.7 -c pytorch -c nvidia


#### Worked

- Starting clean
- Unistalled cuda, deleted the v environment and cleared pip cache
... I had to re-install cuda to 11.7 and I think the errors installing cuda where from vs code  and then following the  [setup using cuda toolkit 11.7](https://github.com/graphdeco-inria/gaussian-splatting/issues/332) I was able to run it and also not get errors...I also noticed that I had not run a recursive git clone and there was an error in one of the submodules that could not find a library that I saw was downloaded on the recursive download so...maybe that was also a problem

- It takes around 20 mins to train
```
C:\Users\Ana\Documents\Fall23_MAT_200C\viewers\bin>SIBR_gaussianViewer_app.exe -m C:\Users\Ana\Documents\Fall23_MAT_200C\gaussian-splatting\output\train
```

### Getting my images

extract images from video
```
FFMPEG -i {path to video} -qscale:v 1 -qmin 1 -vf fps={frame extraction rate} %04d.jpg
```
- I modified extraction rate to achieve around 200 images 

estimating camera poses
- [Colmap](https://github.com/colmap/colmap) for camera poses and intrinsic parameters
- Steps are explained here: https://github.com/jonstephens85/gaussian-splatting-Windows#preparing-images-from-your-own-scenes
- The convert.py file provided in the GS repository and a colmap installation is what I needed to have the images 
## Rendering Splats

Web viewer:
https://discourse.threejs.org/t/3d-gaussian-splatting-in-three-js/57858
https://github.com/SpectacularAI/splat 
https://github.com/antimatter15/splat 
https://jatentaki.github.io/portfolio/gaussian-splatting/ <- does not work

```
python -m http.server 8000 --bind 127.0.0.1
```

```
http://localhost:8000/?url=http://localhost:8000/data/cup2.splat&z_is_up=true
```
#### Splat files

I need to get a .splat file
https://github.com/mkkellogg/GaussianSplats3D 
In this website I can convert it
https://projects.markkellogg.org/threejs/demo_gaussian_splats_3d.php

This tool also seems useful for getting a splat file: 
https://github.com/SpectacularAI/point-cloud-tools

I have to use the file inside the point cloud folder, for example. If I drop it in the splat project it will be converted
```
C:\Users\Ana\Documents\Fall23_MAT_200C\gaussian-splatting\output\cup\point_cloud\iteration_30000 
```
