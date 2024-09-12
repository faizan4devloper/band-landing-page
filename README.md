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
  faCompress,
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
  const [videoState, setVideoState] = useState("small");
  const [isPlaying, setIsPlaying] = useState(false);
  const [isMuted, setIsMuted] = useState(true);
  const [volume, setVolume] = useState(0.2);
  const videoRef = useRef(null);

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

  const toggleMaximize = () => {
    setVideoState((prevState) => (prevState === "small" ? "big" : "small"));
  };

  useEffect(() => {
    window.addEventListener("scroll", () => {
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
    });
  }, [videoState, isPlaying]);

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
          muted={isMuted}
          onClick={togglePlayPause}
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
            icon={isMuted || volume === "0" ? faVolumeMute : faVolumeUp}
            className={styles.volumeIcon}
            onClick={toggleMute}
            title={isMuted ? "Unmute" : "Mute"}
          />
          <input
            type="range"
            min="0"
            max="1"
            step="0.01"
            value={volume}
            onChange={handleVolumeChange}
            className={styles.volumeSlider}
          />
        </div>

        {/* Maximize/Minimize Button */}
        <button className={styles.maximizeButton} onClick={toggleMaximize}>
          <FontAwesomeIcon
            icon={videoState === "big" ? faCompress : faExpand}
            title={videoState === "big" ? "Minimize" : "Maximize"}
          />
        </button>
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
  transition: transform 0.5s ease, width 0.5s ease, height 0.5s ease;
  overflow: hidden;
}

.videoContainer.small {
  width: 300px;
  height: 200px;
  transform: translateY(0);
  bottom: 20px;
  right: 20px;
  position: fixed;
  z-index: 1000;
}

.videoContainer.big {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 999;
}

.video {
  width: 100%;
  height: 100%;
}

.playPauseButton, .maximizeButton {
  position: absolute;
  bottom: 20px;
  left: 20px;
  z-index: 1001;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
}

.volumeControl {
  position: absolute;
  bottom: 20px;
  right: 20px;
  display: flex;
  align-items: center;
  z-index: 1001;
}

.volumeIcon {
  cursor: pointer;
}

.volumeSlider {
  margin-left: 10px;
  cursor: pointer;
}

.maximizeButton {
  right: 70px;
}

.pulse {
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.2); }
  100% { transform: scale(1); }
}
