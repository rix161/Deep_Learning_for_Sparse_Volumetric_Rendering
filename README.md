# Deep Learning Approach to Sparse Volumetric Rendering  

Naive raycasting method to direct volume rendering can be compute intensive and is a significant bottleneck when rending large volumetric data. Several approches have been explored to improve the rendering performace such as early termination, octree representation of volume space and space skipping.

[SparseLeap](https://ieeexplore.ieee.org/document/8017589) is a hybrid order space skipping algorithm which combines the advantage of both image-order and object-order approches to volume rendering. The algorithm categorises the ray passing through the volume into a ray segments, the ray segments is labelled as empty or non-empty. During the rendering process, ray segments corresponding to empty category can be ignored. 

<p align="center">
  <img src="https://github.com/rix161/Deep_Learning_for_Sparse_Volumetric_Rendering/blob/master/images/1_RC_VS_SL.png" alt="RC_VS_SL"/>
</p>

Key take away from the algorithm is that we can gauge the usefulness of the ray before the rendering, As such we could choose to completely discard the ray depending on the the usefullness of the ray. However, the rendered 2D projection would be of low fidility.

<p align="center">
  <img src="https://github.com/rix161/Deep_Learning_for_Sparse_Volumetric_Rendering/blob/master/images/2_RC_VS_SRC.png" alt="Sparse_Volume_Render"/>
</p>

<p align="center">
  <img src="https://github.com/rix161/Deep_Learning_for_Sparse_Volumetric_Rendering/blob/master/images/4_sample_sparse.png" alt="Sample_Sparse_Rendering"/>
Volume rendering with discarded ray segments
</p>

Improving the quality of the low fidility image can be viewed as a denoising problem. Hence, we take a deep learning approch to denoise the image by training a autoencoder with skip connections.

### Training Data
The training was done using image patches of 128x128.

### Architecture
As we would like to infer on images of arbitrary sizes, I keep the network to be fully convolutional network. The network consists of 5 layers of convolution layers with a compression factor of 4 and 5 layers of de-convolution layer with decompression.Additionally there are skip connections between the corresponding encoder and decoder layers. 
  - Encoder Layer 3X3 convolution Stride: 2 padding:1  
  - Decoder Layer 4x4 convolution stride:2 padding:1   
  - ADAM Optimizer.
  - L1 Loss function.
  
<p align="center">
  <img src="https://github.com/rix161/Deep_Learning_for_Sparse_Volumetric_Rendering/blob/master/images/3_Archi.png" alt="Sample_Sparse_Rendering"/>
</p>

### Results
<p align="center">
  <img src="https://github.com/rix161/Deep_Learning_for_Sparse_Volumetric_Rendering/blob/master/images/8_Result.png" alt="Sample_Sparse_Rendering"/>
</p>