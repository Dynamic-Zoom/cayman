---
layout: default
---

<div id="overview-wrapper" hidden>
<h2 id="overview">Project Overview</h2>
<p>This project is developed as part of a project for Computer Vision (CS/ECE 766) at University of Wisconsin-Madison during Spring 2024.</p>
<ul>
  <li><strong>Team Members:</strong> Shantanu Vichare, Dario Placencio, Nico Ranabhat, Hemanth Sridhar Nakshatri</li>
  <li><strong>Course:</strong> CS/ECE 766 - Computer Vision</li>
  <li><strong>Presentation link:</strong> 
  <a href="assets/Dynamic_Zoom_Project_Presentation.pptx" download>Download Presentation</a>
  <!-- <a href="https://uwprod-my.sharepoint.com/:p:/g/personal/nakshatri_wisc_edu/EVOCzj17TrFIu-f7eACxn28BIXpDpTGRlnAQ17ygtrZDTg">OneDrive link</a> -->
  (Note: large file - takes time to download) </li>
  <li><strong>GitHub Repository link:</strong> 
  <a href="{{ site.github.repository_url }}">{{ site.github.repository_url }}</a></li>
</ul>
</div>

## Welcome to Dynamic Zoom
Dynamic Zoom is designed to enhance video resolution in real time, transforming how digital media is consumed and produced across various industries. Leveraging cutting-edge super-resolution technologies, this project addresses the common degradation of video quality during digital zoom, such as blurring and pixelation, which affects viewer satisfaction and hampers professional digital analysis. By integrating advanced machine learning algorithms and computational strategies, Dynamic Zoom aims to deliver high-definition content without the substantial processing time associated with traditional super-resolution methods. 

<div style="padding:66.2% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/941358351?badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="transition"></iframe></div>


## Motivation

The need for high-definition, clear imagery is more critical than ever across various fields:

- **Sports Analysis:** Enhancing video quality for clearer slow-motion replays.
- **Augmented Reality:** Improving the resolution for more immersive experiences.
- **Security and Surveillance:** Increasing video clarity for better monitoring and analysis.
- **Search and Rescue Operations:** Enhancing video details to aid critical missions.

## Challenges and Innovation

Digital zoom often results in significant quality degradation, such as pixelation, like the following example: 

<p align="center">
  <img src="assets/pictures/optical1.jpg" alt="optical vs digital zoom"/>
</p>

Our project, Dynamic Zoom, aims to address these issues by providing a real-time solution that not only enhances clarity but also operates efficiently. This is based on the application of advanced super-resolution techniques limited to the key Regions of Interest (ROIs) in the video frames.

<p align="center">
  <img src="assets/pictures/optical2.jpg" alt="region of interest example"/>
</p>

## Technical Insight

Our approach utilizes the Bicubic++ model, highly awarded for its efficiency and effectiveness in enhancing low-resolution images in real time. This model selection is based on its ability to generate high-quality images with minimal computational resources, when comparing to other advanced super-resolution methods like SwiftsSRGAN, that while powerful, do not offer the speed necessary for real-time applications.

### Bicubic++ Model

<p align="center">
  <img src="assets/pictures/bicubic_model_parametric.svg" alt="bicubic model"/>
</p>

Bicubic++ is an advanced super-resolution model using deep learning and convolutional neural networks (CNNs) to enhance video and image quality. Trained on a broad dataset of image pairs, it efficiently learns to upscale images while preserving detail and texture. Key features include PixelShuffle for effective resolution enhancement and smoother pixel calculation functions to optimize image sharpness. Utilized by Dynamic Zoom, Bicubic++ ensures high-quality video output in real time, ideal for performance-critical applications.

<p align="center">
  <img src="assets/pictures/bicubic_func_comparison.svg" alt="function comparison"/>
</p>


## Methodology

Dynamic Zoom combines GPU-accelerated processing with advanced machine learning techniques:

### PyTorch and CUDA Integration

- **High-Performance Computing:** By integrating PyTorch with CUDA, Dynamic Zoom leverages the parallel computing capabilities of NVIDIA GPUs. This integration allows for the rapid execution of multiple operations simultaneously, drastically reducing the time required for processing video frames.
- **Optimized Resource Utilization:** CUDA's advanced memory management techniques help in efficiently handling large video datasets, ensuring that GPU resources are optimally utilized to speed up computation without bottlenecks.
- **Scalable Architecture:** The flexibility of PyTorch coupled with the robustness of CUDA enables Dynamic Zoom to scale across different hardware configurations effortlessly, ensuring consistent performance regardless of the complexity or size of the video data being processed.

### Advanced Upscaling Techniques: PixelShuffle and Conv2D Transpose

<p align="center">
  <img src="assets/pictures/conv2d.gif" alt="2d convolution"/>
</p>

**Conv2D Transpose:**
  - **Spatial Expansion:** Conv2D Transpose, often referred to as deconvolution, is used to perform spatial upsampling of the video frames. This method allows Dynamic Zoom to expand the spatial dimensions of a feature map, thus generating a larger output that retains much of the original's detail and structure.
  - **Real-Time Processing:** By adjusting the stride and padding parameters, Conv2D Transpose layers can be tuned to produce high-resolution outputs from lower-resolution inputs quickly, which is crucial for maintaining real-time performance during live video enhancements.

<p align="center">
  <img src="assets/pictures/pixelshuffle.gif" alt="pixel shuffle"/>
</p>

**PixelShuffle:**
  - **Resolution Enhancement:** PixelShuffle is employed to rearrange the output of convolution layers, effectively increasing the resolution of images. It works by shuffling pixel values from a low-resolution grid to a high-resolution matrix, enabling finer details to be reconstructed with greater accuracy.
  - **Efficient Feature Learning:** Unlike traditional upscaling methods that may blur finer details, PixelShuffle preserves textural nuances and edges, making it ideal for enhancing video quality in real-time applications.
  
These sophisticated techniques enable Dynamic Zoom to not only upscale video resolutions effectively but also to ensure that the output is of high quality, with enhanced clarity and sharpness, suitable for various real-time applications such as broadcast media, surveillance, and AR/VR environments.

## Interactive Video Enhancement Pipeline

Our system is designed for user engagement and quality output, structured as follows:

1. **InputStream:** This component handles incoming video streams from various sources, such as live cloud streams or recorded file streams. It preprocesses these frames, including resizing and tensor conversion, to prepare them for super-resolution processing. Additionally, it captures the user-defined area of focus (ROI), which is critical for tailoring the enhancement process to the user’s specific interests.

2. **FrameBuffer (InputStream → ModelExecutor):** The frame buffer serves as a temporary holding area for preprocessed frames before they are fed into the ModelExecutor. This buffering is crucial for maintaining smooth processing, especially when the rates of incoming video and processing do not align. It supports parallel processing by decoupling stream handling from model execution, ensuring efficiency and speed.

3. **ModelExecutor:** At the heart of our system, the ModelExecutor employs advanced machine learning models to upscale and enhance the quality of the cropped images from the InputStream. It operates within the controlled environment provided by the input and output frame buffers, optimizing the super-resolution process to produce high-quality video frames in real time.

4. **FrameBuffer (ModelExecutor → OutputStream):** Post-processing, this frame buffer holds the enhanced frames until they are ready to be outputted or saved. It utilizes CUDA pinned memory technology to optimize the transfer of upscaled frames from the GPU to the CPU, enhancing the efficiency of data handling between different system components.

5. **OutputStream:** This module manages the output stream, incorporating GPU acceleration to render the super-resolution output on the fly. It ensures that the enhanced video is displayed to the user without delay, providing an immediate improvement in video quality.

6. **FileWriter:** The FileWriter component takes the processed frames from the OutputStream and securely writes them to an offline storage solution. This capability is essential for archiving, distributing, or further analyzing the enhanced videos, as well as conducting detailed quality evaluations.

<p align="center">
  <img src="assets/pictures/pipeline.svg" alt="pipeline"/>
</p>

## Demonstrations and Visuals

### Dynamic Zoom in Action

Before and After using Dynamic Zoom:


<div align="center">
<iframe src="https://player.vimeo.com/video/941340616?badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" width="640" height="360" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write" title="quality" allowfullscreen></iframe></div>

*Click above to watch Dynamic Zoom enhance video quality in real time!*

*Note the enhanced clarity and detail in the 'Bicubic++' image.*

When comparing just the upscaling quality:

<p align="center">
  <img src="assets/pictures/upscalercomparison.png" alt="upscaler comparison"/>
</p>

*The **Bicubic++** model excels in preserving image details and sharpness.*

### Latency Comparison

Dynamic Zoom's real-time processing capabilities:

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/941360142?badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="latency performance"></iframe></div>

*The system achieves high-quality results with minimal delay.*

## Results

Our system has demonstrated exceptional capabilities in enhancing video resolution efficiently:

- Our Bicubic++ based approach was able to achieve results that were close to Swift SR-GAN in terms of *Peak Signal to Noise Ratio* and *Structural Similarity Index Measure*. 
- Further, Bicubic++ achieved high-quality results with inference speeds of about 7-8 ms for 720p to 4K upscaling. In contrast, Swift SR-GAN 2x and 4x models had inference speeds of atleast 200-250 ms demonstrating the effectiveness of Bicubic++ model.
- Detailed results are shown in the graph.

<p align="center">
  <img src="assets/pictures/results_graph.png" alt="results"/>
</p>

<!-- - **From 360p to 1080p and 720p to 4K:** Achieving high-quality results without noticeable delay.
- **Performance Metrics:**
  - **Processing Speed:** 4.4 milliseconds per frame.
  - **SSIM:** 0.972, indicating excellent structural integrity.
  - **PSNR:** 43.67 dB, demonstrating superior image clarity. -->

## Demo: Sports Streaming

Dynamic Zoom can help enhance sports broadcasts by improving clarity and detail during live events. The video below demonstrates how this technology provides a more clear zoom within a basketball highlight clip.

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/942181648?badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="Dynamic Zoom Application"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>

## Future Directions

We are exploring several enhancements to further improve Dynamic Zoom:

- **Advanced Parallel Processing:** To overcome limitations related to the Global Interpreter Lock (GIL).
- **Exploration of Larger Models:** For even better quality, potentially trading off some inference time for upscaling quality.
- **Compression Artifact Suppression:** The Bicubic++ model is a very light weight model which is not able to effectively super resolve/eliminate the artifacts caused by the compression artifacts in lower-quality videos. A temporal-aware architecture focused on speed could be a potenital solution for this.

## Conclusion

Dynamic Zoom aims to provide a meaningful effort in real-time video processing, providing enhanced resolution for various applications that demand high-quality video output. This tool improves video clarity and detail effectively, making it a practical solution for professional and personal uses alike. By leveraging advanced super-resolution techniques and efficient processing strategies, Dynamic Zoom offers a little glimpse into the future of real-time video enhancement, transforming how digital media is consumed and produced across industries.

## References and Further Reading

- Bilecen, Bahri Batuhan, and Mustafa Ayazoglu. "Bicubic++: Designing an Industry-Grade Super-Resolution Network." 2023.
- Krishnan, Koushik S., and Karthik S. Krishnan. "SwiftSRGAN: Rethinking Super-Resolution for Real-Time Inference." 2021.

## Image and Video references

Find all on our repo: [Dynamic-Zoom/home](https://github.com/Dynamic-Zoom/home/blob/master/README.md#references)
