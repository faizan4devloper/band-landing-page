
::-webkit-scrollbar {
  width: 4px;
}


::-webkit-scrollbar-thumb {
  background: #5f1ec1; 
  border-radius: 10px;
}

html, body {
  font-family: "Poppins", sans-serif;

}

.app {
  width: 1100px;
  margin: 0 auto;
  
}

.cardsContainer {
  gap: 20px;
  margin-top: 80px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap; /* Allow wrapping */
  
}

.arrow {
  cursor: pointer;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  font-size: 18px;
  width: 18px;
  height: 18px;
  padding: 5px 5px 5px 5px;
  border-radius: 50px;
  border: 2px solid rgba(15, 95, 220, 1);
  color: rgba(15, 95, 220, 1);
  transition: transform 0.5s ease, background 0.5s ease;
}

.arrow:hover {
  background-color: rgba(15, 95, 220, 1);
  color: white;
}

.leftArrow {
  left: -25px;
  top: 90px;
}

.rightArrow {
  right: -25px;
  top: 90px;
}

@media screen and (max-width: 1100px) {
  .app {
    padding: 0 10px;
  }
}

@media screen and (max-width: 768px) {
  .app {
    max-width: 100%;
  }
}

.viewAllContainer {
  width: 100%;
  display: flex;
  justify-content: right;
  align-items: center;
  margin-right: 112px;
}

.solutionHead {
  font-weight: 600;
  font-size: 14px;
  color: #808080;
  margin-left: 118px;
}

.viewAllButton {
  font-weight: 600;
  font-size: 14px;
  color: #808080;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease, color 0.3s ease;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  padding: 6px 7px;
  border-radius: 5px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
}

.viewAllButton:hover {
  transform: translateY(-5px);
  color: #5f1ec1;
  background-color: rgba(13, 85, 198, 0.1);
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.2);
}

.icon {
  margin-left: 8px;
  transition: transform 0.3s ease;
}

.viewAllButton:hover .icon {
  transform: translateX(5px);
}

.scrollDownButton,
.scrollUpButton {
  position: fixed;
  left: 20px;
  bottom: 20px;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  border-radius: 4px;
  width: 21px;
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.scrollDownButton:hover, .scrollUpButton:hover {
  background-color: rgba(13, 85, 198, 1);
}

/* Add this to your existing styles */
.loader {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh; /* Full height */
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: rgba(255, 255, 255, 0.9); /* Semi-transparent background */
  z-index: 1000; /* Make sure loader appears above other content */
}

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
  background-color: rgba(95, 30, 193, 0.8);
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
  background-color: rgba(95, 30, 193, 1);
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


/* Desktop and large screens */
@media screen and (min-width: 1024px) {
  .app {
    width: 1100px;
    margin: 0 auto;
  }

  .allCardsPage {
    padding: 20px;
    margin-top: 112px;
    margin-left: 270px;
    position: fixed;
  }

  .cardsContainer {
    gap: 20px;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
  }
}

/* Tablet screens */
@media screen and (min-width: 768px) and (max-width: 1024px) {
  .app {
    width: 100%;
    padding: 0 20px;
  }

  .allCardsPage {
    margin-left: 220px;
  }

  .cardsContainer {
    grid-template-columns: repeat(3, 1fr);
    
  }

  .sidebar {
    width: 200px;
  }
}

/* code for ChatBot */





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
      videoRef.current.muted = !isMuted; // Toggle mute state
      setIsMuted(!isMuted);
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleDebouncedScroll);

    return () => {
      window.removeEventListener("scroll", handleDebouncedScroll);
    };
  }, [handleDebouncedScroll]);

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
          muted={isMuted} // Video starts muted
          onClick={togglePlayPause} // Allow play/pause by clicking on video
          onCanPlay={() => videoRef.current.play()} // Ensure video plays when it can
        />
        <button
          className={`${styles.playPauseButton} ${!isPlaying ? "pulse" : ""}`}
          onClick={togglePlayPause}
        >
          <FontAwesomeIcon icon={isPlaying ? faPause : faPlay} />
        </button>

        {/* Mute/Unmute button */}
        <button
          className={styles.muteButton}
          onClick={toggleMute}
        >
          <FontAwesomeIcon icon={isMuted ? faVolumeMute : faVolumeUp} />
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









import React, { useState, useEffect, useRef } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faPlay, faPause } from "@fortawesome/free-solid-svg-icons";
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

  const unmuteVideo = () => {
    if (videoRef.current) {
      videoRef.current.muted = false; // Unmute the video
      setIsMuted(false);
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleDebouncedScroll);

    return () => {
      window.removeEventListener("scroll", handleDebouncedScroll);
    };
  }, [handleDebouncedScroll]);

  useEffect(() => {
    // Handle unmuting video when user interacts
    const handleUserInteraction = () => {
      if (isMuted) {
        unmuteVideo();
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
          onCanPlay={() => videoRef.current.play()} // Ensure video plays when it can
        />
        <button
          className={`${styles.playPauseButton} ${!isPlaying ? "pulse" : ""}`}
          onClick={togglePlayPause}
        >
          <FontAwesomeIcon icon={isPlaying ? faPause : faPlay} />
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









ERROR
play() failed because the user didn't interact with the document first. https://goo.gl/xX8pDD

import React, { useState, useEffect, useRef } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faPlay, faPause } from "@fortawesome/free-solid-svg-icons";
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

  useEffect(() => {
    window.addEventListener("scroll", handleDebouncedScroll);

    return () => {
      window.removeEventListener("scroll", handleDebouncedScroll);
    };
  }, [handleDebouncedScroll]);

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
          onClick={togglePlayPause} // Allow play/pause by clicking on video
          onCanPlay={() => videoRef.current.play()} // Ensure video plays when it can
        />
        <button
          className={`${styles.playPauseButton} ${!isPlaying ? "pulse" : ""}`}
          onClick={togglePlayPause}
        >
          <FontAwesomeIcon icon={isPlaying ? faPause : faPlay} />
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









