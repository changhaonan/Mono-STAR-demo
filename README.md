# Mono-STAR: Mono-camera Scene-level Tracking and Reconstruction

## Overview

We present **Mono-STAR**, the first real-time 3D reconstruction system  that simultaneously supports semantic fusion, fast motion tracking, non-rigid object deformation, and topological change under a unified framework.
The proposed system solves a new optimization problem that incorporates  optical-flow-based 2D constraints to deal with fast motion, and a novel semantic-aware deformation graph (SAD-graph) for handling topology change. We test the proposed system under various challenging scenes and demonstrate that it significantly outperforms existing state-of-the-art systems.

## Proposed

Overview of the proposed system. The system runs in two parallel threads, one for measurement and one for geometry. In each time-step $t$, the measurement thread loads a measurement $M_t$ from images or a camera buffer. Then, a segmentation network generates a set of semantic labels $L^m_{t}$. Once the measurement is loaded on the GPU memory, $M_t$ and previous alignment rendering $R^a_{t-1}$ are fed into an optical-flow network to generate the optical-flow $OF_t$ from previous geometry $S_{t-1}$ to measurement $M_t$.  Optical-flow $OF_t$, geometry rendering $R_t$ and measurement $M_t$ are used to compute warp-field $W_t$ with non-rigid alignment. After the alignment, previous geometry $S_{t-1}$ will be warped to $S_{t-1}^{warp}$. The fusion render map $R^g_{t-1}$ is then rendered from $S_{t-1}^{warp}$.  $R^g_{t-1}$, $S_{t-1}^{warp}$ are then combined with semantic labels $L^m_t$ from the measurement thread to generate the updated geometry $S_t$. In the end, a new alignment rendering $R^a_t$ is generated from updated geometry $S_t$ for the processing of the next frame.

<img src="docs/Multi-threading_v3.png"/>

## Experiment Results

### Fast Motion - BasketBall

Color | Graph | Semantic
:-: | :-: | :-:
<video src="https://user-images.githubusercontent.com/16898838/194716773-48285025-e2ad-46f0-a4e3-3d7460094a83.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194716784-1b319003-211f-47b5-97d5-257a8846109f.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718086-be884697-0349-4503-aa42-6f2a2aa445e6.mp4" height=100/> 

### Fast Motion - Falling Cup

Color | Graph | Semantic
:-: | :-: | :-:
<video src="https://user-images.githubusercontent.com/16898838/194718250-d8dee217-6eb7-4a8b-b8f9-cb0d1580e1bd.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718260-81771d1f-cbc9-4855-bfe7-906fc8ca89de.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718276-ae0661dc-4547-4a99-a0f2-253cca39a622.mp4" height=100/>

### Topology Change - Pick Cup

Color | Geometry | Semantic
:-: | :-: | :-:
<video src="https://user-images.githubusercontent.com/16898838/194718474-127b6c9d-c9b8-409f-9e50-5f5250c7b329.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718497-35863b64-8f7f-41a2-b978-4230f150b4a6.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718483-6f5464c6-7475-4558-b156-77ab928cf11b.mp4" height=100/>

### Topology Change - Move Toy

Color | Geometry | Semantic
:-: | :-: | :-:
<video src="https://user-images.githubusercontent.com/16898838/194718575-836fe146-be3c-4585-8cff-ee01c1735a21.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718587-3a1779d4-f1ed-4116-a3dd-f8cea451a961.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718601-6502b874-32e8-4cd7-ab56-0fb5e337fee8.mp4" height=100/>

### Topology Change - Push Coffee

Color | Geometry | Semantic
:-: | :-: | :-:
<video src="https://user-images.githubusercontent.com/16898838/194718840-4c65f8db-e0bf-49eb-93be-54e13dde8500.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718848-4b811c4b-a49a-4825-ad0d-7c0d89ec3576.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718860-6653c119-369e-42f3-b6bf-6ac26fd6aedc.mp4" height=100/>

### Deformation - Deform Pillow

Color | Geometry | Graph 
:-: | :-: | :-:
<video src="https://user-images.githubusercontent.com/16898838/194718650-9387f09b-6c46-4240-ac53-631a0f196006.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718673-dbbdde28-9af3-4369-bdd0-c3002cd953f9.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718657-4033af66-e2d1-476f-9968-f69d9f074d25.mp4" height=100/> 

### Deformation - Deform Umbrella

Color | Geometry | Graph 
:-: | :-: | :-:
<video src="https://user-images.githubusercontent.com/16898838/194718761-002c96c7-dde0-49ed-bbed-bac06a2c8cc9.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718770-6893b3c3-687f-47b1-a98d-2fe44c9fae09.mp4" height=100/> | <video src="https://user-images.githubusercontent.com/16898838/194718778-5ee738b6-a102-4279-b350-ac318afc3cad.mp4" height=100/> 

## Conclusion

We presented Mono-STAR, a single-view solution for the semantic-aware STAR problem. Mono-STAR uses a novel semantic-aware and adaptive deformation graph for simultaneous tracking and reconstruction, and can handle topology changes as well as semantic fusion. Experiments show that Mono-STAR achieves promising results in non-rigid object reconstruction, while resisting to semantic segmentation errors, and capturing fast motions on various challenging scenes. We believe that this system can inspire and boost more future research on imitation learning, dexterous manipulation, and many other relevant robotics problems.
