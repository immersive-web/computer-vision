# Synchronous Access to Video Frames for Rendering

Synchronous access to video frames from the camera in GPU memory, particularly on video-mixed-reality devices (e.g., phones running ARKit/ARCore). The scenario here is to support applications creating visual effects by having access to the video in GPU memory, so the simple "overlay graphics on video" approach can be augmented with shadows, reflection, distortion and other effects. It should be possible to implement more efficiently than would be possible if the video frames were first loaded into Javascript buffers, and there is the possibility to address privacy concerns if the implementation can arrange for GPU memory to not to be accessible in Javascript. 

