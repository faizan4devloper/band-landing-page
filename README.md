const handleScroll = () => {
  if (videoRef.current) {
    const videoPosition = videoRef.current.getBoundingClientRect().top;
    const triggerPoint = window.innerHeight / 2;

    if (videoPosition <= triggerPoint && videoState !== "big") {
      setVideoState("big");
      if (!isPlaying) {
        videoRef.current.play();
        setIsPlaying(true);
      }
    } else if (videoPosition > triggerPoint + 100 && videoState !== "small") {
      setVideoState("small");
      if (isPlaying) {
        videoRef.current.pause();
        setIsPlaying(false);
      }
    }
  }
};



.videoContainer {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px 0;
  transition: transform 0.6s ease, width 0.6s ease, height 0.6s ease, opacity 0.6s ease;
  opacity: 0;
  width: 100%; /* Ensure it spans full width */
  height: 50vh; /* Adjust height as needed, using viewport height for responsiveness */
}

.videoContainer.small {
  transform: translateX(0) scale(0.5);
  opacity: 1;
}

.videoContainer.big {
  transform: translateX(0) scale(1);
  opacity: 1;
}

.video {
  width: 100%;
  height: 100%; /* Make the video take the full height of the container */
  object-fit: cover; /* Ensure the video covers the container while maintaining aspect ratio */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  cursor: pointer;
}


.videoContainer {
  position: relative;
  width: 100%;
  height: 100vh; /* Full height of the viewport */
  overflow: hidden;
}

.video {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100%;
  height: 100%;
  transform: translate(-50%, -50%);
  object-fit: cover;
  z-index: -1; /* Ensure content is above the video */
}
