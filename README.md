# Computer vision for XR


This is a feature repo (as defined in the proposals process) that has been created for exploring computer vision APIs for XR.

The conversation that led to the creation of this repo can be found in [Proposals Issues #4](https://github.com/immersive-web/proposals/issues/4).

Blair MacIntyre (@blairmacintyre) is coordinating the creation of the initial explainers.

# Top Level Documents in this Repository

The topic of _computer vision support_ is broad, and implementing support on the web, even just within the context of devices supported by the WebXR Device API, could be approached in a number of different ways.  In this repository, we would like to capture and document the various ways this could be thought about, even though the initial proposal will focus on a subset of the possible approaches and use cases.

There are 5 potential use cases we could consider working on, intertwined but sufficiently separate we can talk about them separately. These will be documents in separate explainers.


- Synchronous access to video frames from the camera in GPU memory, particularly on video-mixed-reality devices (e.g., phones running ARKit/ARCore). The scenario here is to support applications creating visual effects by having access to the video in GPU memory, so the simple "overlay graphics on video" approach can be augmented with shadows, reflection, distortion and other effects. It should be possible to implement more efficiently than would be possible if the video frames were first loaded into Javascript buffers, and there is the possibility to address privacy concerns if the implementation can arrange for GPU memory to not to be accessible in Javascript. Documented in [video-effects.md](video-effects.md)

 - Asynchronous access to video frames in the CPU and/or GPU. Asynchronous because most non-video-mixed devices do not run the camera and display at the same frame rates, so synchronized access may not be practical. The scenario here is to do real-time computer vision (e.g., SLAM like 6d.ai, 8thWall, etc. are working on; CV tracking algorithms like Vuforia; face and object detection) in a platform independent way. Some of what will be done here might eventually make it into platforms (and may already exist in some platforms), such as image detection. Others would include custom algorithms that need to work everywhere, to support web applications for art, advertising, games, and so on. Documented in [cv-in-page.md](cv-in-page.md)

 - Extending WebRTC to support access to cameras on XR device sessions, and streaming/recording of video from XR devices. One goal of this approach is to avoid duplicating functionality across Web APIs (e.g., keep video device access in one place).  The core scenario here is "a worker wants to allow an expert to see what they are seeing, and overlay augmentations in their view." This scenario applies to enterprise scenarios as well as consumer apps. For consumers, this is "home owner wants to show Home Depot consultant something, get guidance and order parts for repair". This work will likely need to be done over in the WebRTC WG, as they work on the next version of WebRTC, but we should track it here in a document with relevant pointers to issues and capabilities. Documented in [webrtc-remote-video.md](webrtc-remote-video.md)

- Exposing native, cross platform CV algorithms. The browser can expose entire algorithms running on the camera video, as suggested by the [shape-detection api proposal](https://wicg.github.io/shape-detection-api/).  We could start with very basic capabilities (like detecting barcodes in 3D, images, perhaps faces). Some of the specific algorithms could be optional, but it would be nice if there were very straightforward things (like barcodes, discussed in the shape-detection api) that could be implemented everywhere. If video frames are available inside the web page, it may be possible to polyfill them with WASM implementations.  Developers and users would benefit strongly from having some common capabilities that are implemented natively when possible (e.g., using ARKit for image detection on iOS), and having an agreed upon approach for exposing platform-specific capabilities that might be hard to implement in WASM (like ARKit/ARCore's more advanced features). Documented in [cv-in-browser.md](cv-in-browser.md)

- Exposing native computer vision building blocks and/or components, in a way that leverages native processing of video frames before sending the pre-processed video frames (and associated data) into the web app (either into the GPU or CPU), perhaps leveraging a library like [Khronos' OpenVX](https://www.khronos.org/openvx/). (In fact, one way this could be thought about is by working with Khronos to define WebVX.) Being able to leverage optimized platform capabilities for doing well-known basic algorithms (image pyramids, simple feature extraction, image conversion, blur, etc) could speed up in-app CV, and also allow some of the effects that might be done in the synchronous GPU case to be done faster. Like the WebRTC discussion, if we wanted to pursue this, we would want to do that elsewhere, but the scenario has been brought up multiple times, so we should document, summarize and record it it here. Documented in [webvx.md](webvx.md).

The first two are the focus of this repository, and share a common need to have the available cameras exposed and the developer request access to them. They can likely be done in the same API (e.g., request cameras, direct data to CPU and/or GPU, guarantee that if the camera is synch with rAF that the data will be available before rAF and make this known if so), but are separable if we only want to tackle one first. 