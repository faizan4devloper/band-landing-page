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



import React, { Suspense, lazy } from "react";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = lazy(() => import("./Card"));

const Cards = ({ imageUrl, title, description, isBig, toggleSize, tags }) => {
  const [isHovered, setIsHovered] = React.useState(false);
  const [showPopup, setShowPopup] = React.useState(false);

  React.useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  // Merge tags into a single string separated by "/"
  const mergedTags = tags && tags.length > 1 ? tags.join(" / ") : tags?.[0];

  // List of titles where the popup should be shown instead of navigating
  const noReadMoreTitles = [
    "AAIG-API Analyzer & Insight Generator",
    "Responsible Gen AI with Llama-13 B",
    "Graph data Interpretation using Gen AI",
    "Predictive Asset Maintenance​(PAM)",
    "API based Test Case Generation",
    "SOP Assistance",
    "AMS Support Automation",
  ];

  const handleReadMoreClick = (e) => {
    if (noReadMoreTitles.includes(title)) {
      e.preventDefault(); // Prevent navigation
      setShowPopup(true); // Show the pop-up
    }
  };

  return (
    <div className={styles.cardsContainer}>
      <Suspense fallback={<div>Loading Card...</div>}>
        <Card
          imageUrl={imageUrl}
          title={title}
          description={description}
          isBig={isBig}
          toggleSize={toggleSize}
          tags={tags}
          handleReadMoreClick={handleReadMoreClick}
          showPopup={showPopup}
          setShowPopup={setShowPopup}
          isHovered={isHovered}
          setIsHovered={setIsHovered}
        />
      </Suspense>
    </div>
  );
};

export default Cards;
