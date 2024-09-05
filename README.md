import React, { useState, useEffect, useRef } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faPlay, faPause, faVolumeUp, faVolumeMute } from "@fortawesome/free-solid-svg-icons";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import { Link } from "react-router-dom";

const S3_VIDEO_URL = "https://aiml-convai.s3.amazonaws.com/portal-bg-video/BgVideos.webM";

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
  const [volume, setVolume] = useState(0.2); // Set default volume to 20%
  const videoRef = useRef(null);

  const handleScroll = () => {
    if (videoRef.current) {
      const videoPosition = videoRef.current.getBoundingClientRect().top;
      const triggerPoint = window.innerHeight / 2;

      if (videoPosition <= triggerPoint && videoState !== "big") {
        setVideoState("big");
        if (!isPlaying) {
          videoRef.current.play().catch(error => {
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
      try {
        if (isPlaying) {
          videoElement.pause();
        } else {
          videoElement.play().catch(error => {
            console.error("Error during video playback:", error);
          });
        }
        setIsPlaying(!isPlaying);
      } catch (error) {
        console.error("Error during video playback:", error);
      }
    } else {
      console.error("Video element is not available or not properly referenced.");
    }
  };

  const toggleMute = () => {
    if (videoRef.current) {
      const isCurrentlyMuted = videoRef.current.muted;
      videoRef.current.muted = !isCurrentlyMuted; // Toggle mute/unmute
      setIsMuted(!isCurrentlyMuted);
    }
  };

  const handleVolumeChange = (event) => {
    const newVolume = event.target.value;
    setVolume(newVolume);
    if (videoRef.current) {
      videoRef.current.volume = newVolume;
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
      videoRef.current.volume = 0.2; // Set the default volume to 20%
    }
  }, []);

  useEffect(() => {
    // Handle unmuting video when user interacts
    const handleUserInteraction = () => {
      if (isMuted) {
        toggleMute(); // Toggle mute/unmute on interaction
      }
    };

    // Add event listener for user interaction
    document.addEventListener('click', handleUserInteraction);

    // Cleanup the event listener when component unmounts
    return () => {
      document.removeEventListener('click', handleUserInteraction);
    };
  }, [isMuted]);

  return (
    <>
      <MyCarousel />
      <div className={`${styles.videoContainer} ${styles[videoState]}`}>
        <video
          className={styles.video}
          src={S3_VIDEO_URL}
          loop
          ref={videoRef}
          playsInline
          muted={isMuted} // Video starts muted, then unmutes after user interaction
          onClick={togglePlayPause} // Allow play/pause by clicking on video
        />
        <button
          className={`${styles.playPauseButton} ${!isPlaying ? "pulse" : ""}`}
          onClick={togglePlayPause}
        >
          <FontAwesomeIcon icon={isPlaying ? faPause : faPlay} />
        </button>

        {/* Volume Controller */}
        <div className={styles.volumeControl}>
          <FontAwesomeIcon
            icon={isMuted ? faVolumeMute : faVolumeUp} // Change icon based on mute state
            className={styles.volumeIcon}
            onClick={toggleMute} // Toggle mute/unmute on click
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
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
};

export default Home;
