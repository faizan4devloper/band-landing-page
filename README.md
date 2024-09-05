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









