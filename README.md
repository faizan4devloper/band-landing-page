import React, { useState, useEffect, useRef, Suspense, lazy } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import {
  faArrowRight,
  faArrowLeft,
  faPlay,
  faPause,
  faVolumeUp,
  faVolumeMute,
  faExpand,
  faCompress
} from "@fortawesome/free-solid-svg-icons";
import styles from "./App.module.css";
import { Link } from "react-router-dom";

const MyCarousel = lazy(() => import("./components/Carousel/MyCarousel"));
const Cards = lazy(() => import("./components/Cards/Cards"));

const S3_VIDEO_URL = "https://aiml-convai.s3.amazonaws.com/portal-bg-video/HCLTech_AWS_GenAI+Solution+Video.mp4";

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  currentIndex,
  bigIndex,
  toggleSize,
  cardsContainerRef,
  handleMouseEnter,
}) => {
  const [videoState, setVideoState] = useState("hidden");
  const [isPlaying, setIsPlaying] = useState(false);
  const [isMuted, setIsMuted] = useState(true); // Start video as muted
  const [volume, setVolume] = useState(0.2); // Default volume
  const [isFullscreen, setIsFullscreen] = useState(false); // State for fullscreen
  const videoRef = useRef(null);

  const handleScroll = () => {
    if (videoRef.current) {
      const videoPosition = videoRef.current.getBoundingClientRect().top;
      const triggerPoint = window.innerHeight / 2;

      if (videoPosition <= triggerPoint && videoState !== "big") {
        setVideoState("big");
        if (!isPlaying) {
          videoRef.current.play().catch((error) => {
            console.error("Error during video playback:", error);
          });
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

  const debounce = (func, wait) => {
    let timeout;
    return (...args) => {
      clearTimeout(timeout);
      timeout = setTimeout(() => func.apply(this, args), wait);
    };
  };

  const handleDebouncedScroll = debounce(handleScroll, 100);

  const togglePlayPause = () => {
    const videoElement = videoRef.current;
    if (videoElement) {
      if (isPlaying) {
        videoElement.pause();
      } else {
        videoElement.play().catch((error) => {
          console.error("Error during video playback:", error);
        });
      }
      setIsPlaying(!isPlaying);
    }
  };

  const toggleMute = () => {
    if (videoRef.current) {
      videoRef.current.muted = !isMuted;
      setIsMuted(!isMuted);
    }
  };

  const handleVolumeChange = (event) => {
    const newVolume = event.target.value;
    setVolume(newVolume);

    if (videoRef.current) {
      videoRef.current.volume = newVolume;
      if (newVolume > 0) {
        videoRef.current.muted = false;
        setIsMuted(false);
      } else {
        videoRef.current.muted = true;
        setIsMuted(true);
      }
    }
  };

  const toggleFullscreen = () => {
    if (!isFullscreen) {
      // Enter fullscreen
      if (videoRef.current.requestFullscreen) {
        videoRef.current.requestFullscreen();
      } else if (videoRef.current.mozRequestFullScreen) {
        videoRef.current.mozRequestFullScreen();
      } else if (videoRef.current.webkitRequestFullscreen) {
        videoRef.current.webkitRequestFullscreen();
      } else if (videoRef.current.msRequestFullscreen) {
        videoRef.current.msRequestFullscreen();
      }
      setIsFullscreen(true);
    } else {
      // Exit fullscreen
      if (document.exitFullscreen) {
        document.exitFullscreen();
      } else if (document.mozCancelFullScreen) {
        document.mozCancelFullScreen();
      } else if (document.webkitExitFullscreen) {
        document.webkitExitFullscreen();
      } else if (document.msExitFullscreen) {
        document.msExitFullscreen();
      }
      setIsFullscreen(false);
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleDebouncedScroll);

    return () => {
      window.removeEventListener("scroll", handleDebouncedScroll);
    };
  }, [handleDebouncedScroll]);

  useEffect(() => {
    // Set default volume when the component mounts
    if (videoRef.current) {
      videoRef.current.volume = volume;
    }
  }, [volume]);

  return (
    <>
      <Suspense fallback={<div>Loading Carousel...</div>}>
        <MyCarousel />
      </Suspense>
      <div className={`${styles.videoContainer} ${styles[videoState]}`}>
        <video
          className={styles.video}
          src={S3_VIDEO_URL}
          loop
          ref={videoRef}
          playsInline
          muted={isMuted} // Control video mute/unmute state
          onClick={togglePlayPause} // Allow play/pause by clicking on video
        />
        <button
          className={`${styles.playPauseButton} ${!isPlaying ? "pulse" : ""}`}
          onClick={togglePlayPause}
        >
          <FontAwesomeIcon icon={isPlaying ? faPause : faPlay} />
        </button>

        {/* Fullscreen Button */}
        <button className={styles.fullscreenButton} onClick={toggleFullscreen}>
          <FontAwesomeIcon icon={isFullscreen ? faCompress : faExpand} />
        </button>

        {/* Volume Control */}
        <div className={styles.volumeControl}>
          <FontAwesomeIcon
            icon={isMuted || volume === "0" ? faVolumeMute : faVolumeUp} // Toggle icon based on mute state or volume level
            className={styles.volumeIcon}
            onClick={toggleMute} // Mute/unmute on icon click
            title={isMuted ? "Unmute" : "Mute"}
          />
          <input
            type="range"
            min="0"
            max="1"
            step="0.01"
            value={volume}
            onChange={handleVolumeChange} // Change volume on slider change
            className={styles.volumeSlider}
          />
        </div>
      </div>

      <div
        className={styles.cardsContainer}
        ref={cardsContainerRef}
        onMouseEnter={handleMouseEnter}
      >
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
          </Link>
        </div>
        <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
          <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
        </span>
        <Suspense fallback={<div>Loading Cards...</div>}>
          {cardsData.slice(currentIndex, currentIndex + 5).map((card, index) => {
            const actualIndex = currentIndex + index;
            return (
              <Cards
                key={index}
                imageUrl={card.imageUrl}
                title={card.title}
                description={card.description}
                isBig={actualIndex === bigIndex}
                toggleSize={() => toggleSize(actualIndex)}
              />
            );
          })}
        </Suspense>
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
};

export default Home;
















import React, { useState, useEffect, useRef, Suspense, lazy } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faPlay, faPause, faVolumeUp, faVolumeMute } from "@fortawesome/free-solid-svg-icons";
import styles from "./App.module.css";
import { Link } from "react-router-dom";

const MyCarousel = lazy(() => import("./components/Carousel/MyCarousel"));
const Cards = lazy(() => import("./components/Cards/Cards"));

const S3_VIDEO_URL = "https://aiml-convai.s3.amazonaws.com/portal-bg-video/HCLTech_AWS_GenAI+Solution+Video.mp4";

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  currentIndex,
  bigIndex,
  toggleSize,
  cardsContainerRef,
  handleMouseEnter,
}) => {
  const [videoState, setVideoState] = useState("hidden");
  const [isPlaying, setIsPlaying] = useState(false);
  const [isMuted, setIsMuted] = useState(true); // Start video as muted
  const [volume, setVolume] = useState(0.2); // Default volume
  const videoRef = useRef(null);

  const handleScroll = () => {
    if (videoRef.current) {
      const videoPosition = videoRef.current.getBoundingClientRect().top;
      const triggerPoint = window.innerHeight / 2;

      if (videoPosition <= triggerPoint && videoState !== "big") {
        setVideoState("big");
        if (!isPlaying) {
          videoRef.current.play().catch((error) => {
            console.error("Error during video playback:", error);
          });
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

  const debounce = (func, wait) => {
    let timeout;
    return (...args) => {
      clearTimeout(timeout);
      timeout = setTimeout(() => func.apply(this, args), wait);
    };
  };

  const handleDebouncedScroll = debounce(handleScroll, 100);

  const togglePlayPause = () => {
    const videoElement = videoRef.current;
    if (videoElement) {
      if (isPlaying) {
        videoElement.pause();
      } else {
        videoElement.play().catch((error) => {
          console.error("Error during video playback:", error);
        });
      }
      setIsPlaying(!isPlaying);
    }
  };

  const toggleMute = () => {
    if (videoRef.current) {
      videoRef.current.muted = !isMuted;
      setIsMuted(!isMuted);
    }
  };

  const handleVolumeChange = (event) => {
    const newVolume = event.target.value;
    setVolume(newVolume);

    if (videoRef.current) {
      videoRef.current.volume = newVolume;
      if (newVolume > 0) {
        videoRef.current.muted = false;
        setIsMuted(false);
      } else {
        videoRef.current.muted = true;
        setIsMuted(true);
      }
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleDebouncedScroll);

    return () => {
      window.removeEventListener("scroll", handleDebouncedScroll);
    };
  }, [handleDebouncedScroll]);

  useEffect(() => {
    // Set default volume when the component mounts
    if (videoRef.current) {
      videoRef.current.volume = volume;
    }
  }, [volume]);

  return (
    <>
      <Suspense fallback={<div>Loading Carousel...</div>}>
        <MyCarousel />
      </Suspense>
      <div className={`${styles.videoContainer} ${styles[videoState]}`}>
        <video
          className={styles.video}
          src={S3_VIDEO_URL}
          loop
          ref={videoRef}
          playsInline
          muted={isMuted} // Control video mute/unmute state
          onClick={togglePlayPause} // Allow play/pause by clicking on video
        />
        <button
          className={`${styles.playPauseButton} ${!isPlaying ? "pulse" : ""}`}
          onClick={togglePlayPause}
        >
          <FontAwesomeIcon icon={isPlaying ? faPause : faPlay} />
        </button>

        {/* Volume Control */}
        <div className={styles.volumeControl}>
          <FontAwesomeIcon
            icon={isMuted || volume === "0" ? faVolumeMute : faVolumeUp} // Toggle icon based on mute state or volume level
            className={styles.volumeIcon}
            onClick={toggleMute} // Mute/unmute on icon click
            title={isMuted ? "Unmute" : "Mute"}
          />
          <input
            type="range"
            min="0"
            max="1"
            step="0.01"
            value={volume}
            onChange={handleVolumeChange} // Change volume on slider change
            className={styles.volumeSlider}
          />
        </div>
      </div>

      <div
        className={styles.cardsContainer}
        ref={cardsContainerRef}
        onMouseEnter={handleMouseEnter}
      >
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
          </Link>
        </div>
        <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
          <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
        </span>
        <Suspense fallback={<div>Loading Cards...</div>}>
          {cardsData.slice(currentIndex, currentIndex + 5).map((card, index) => {
            const actualIndex = currentIndex + index;
            return (
              <Cards
                key={index}
                imageUrl={card.imageUrl}
                title={card.title}
                description={card.description}
                isBig={actualIndex === bigIndex}
                toggleSize={() => toggleSize(actualIndex)}
              />
            );
          })}
        </Suspense>
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
};

export default Home;


.videoContainer {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px 0;
  transition: transform 0.6s ease-in-out, opacity 0.6s ease-in-out;
  opacity: 0;
  width: 100%;
  height: 95vh;
  transform: translateX(-100%); /* Start hidden to the left */
}

.videoContainer.big {
  transform: translateX(0); /* Slide in from left */
  opacity: 1;
}

.videoContainer.small {
  transform: translateX(-100%); /* Slide out to the left */
  opacity: 0; /* Smoothly disappear */
}

.video {
  width: 100%;
  height: 100%;
  object-fit: fill;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  transition: transform 0.5s ease-in-out, filter 0.5s ease-in-out;
}

.video:hover {
  transform: scale(1.02);
  filter: brightness(1.1);
}

.playPauseButton {
  position: absolute;
  bottom: 20px;
  right: 25px;
  background-color: var(--primary-color);
  color: white;
  border: none;
  padding: 12px 15px;
  cursor: pointer;
  transition: transform 0.3s ease-in-out, background-color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.playPauseButton:hover {
  background-color: var(--button-hover-color);
  transform: scale(1.2); /* Added rotation for a dynamic effect */
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.4);
}

@keyframes pulse {
  0% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }
  50% {
    transform: scale(1.1);
    box-shadow: 0 0 20px rgba(95, 30, 193, 0.5);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }
}

.playPauseButton.pulse {
  animation: pulse 2s infinite;
}
