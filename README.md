import React, { useState, useEffect, useRef } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import { Link } from "react-router-dom";
import BgVideo from "./BgVideos1.mp4";

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
  const videoRef = useRef(null);

  const handleScroll = () => {
    if (videoRef.current) {
      const videoPosition = videoRef.current.getBoundingClientRect().top;
      const triggerPoint = window.innerHeight / 2;

      if (videoPosition <= triggerPoint) {
        setVideoState("big");
      } else if (videoPosition > triggerPoint + 100) {
        setVideoState("small");
      }
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  return (
    <>
      <MyCarousel />
      <div className={`${styles.videoContainer} ${styles[videoState]}`} ref={videoRef}>
        <video
          className={styles.video}
          src={BgVideo}
          controls
          muted
          autoPlay
          loop
        />
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

.videoContainer {
  position: relative;
  display: flex;
  justify-content: center;
  margin: 20px 0;
  transition: transform 0.6s ease, width 0.6s ease, height 0.6s ease, opacity 0.6s ease;
  opacity: 0; /* Initial state: hidden */
}

.videoContainer.hidden {
  transform: translateX(-100%); /* Move off-screen to the left */
}

.videoContainer.small {
  transform: translateX(0) scale(0.5);
  opacity: 1; /* Make visible */
}

.videoContainer.big {
  transform: translateX(0) scale(1);
  opacity: 1; /* Make visible */
}

.video {
  width: 100%;
  height: auto;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  transition: transform 1s ease;
}
