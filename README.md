const togglePlayPause = () => {
  const videoElement = videoRef.current;
  if (videoElement) {
    try {
      if (isPlaying) {
        videoElement.pause();
      } else {
        videoElement.play();
      }
      setIsPlaying(!isPlaying);
    } catch (error) {
      console.error("Error during video playback:", error);
      // Optionally, handle error or notify users
    }
  } else {
    console.error("Video element is not available or not properly referenced.");
  }
};
