import React, { useState, useEffect, useRef } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import { Link } from "react-router-dom";

import BgVideo from "./BgVideos.mp4";

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
  const [videoSize, setVideoSize] = useState("small"); // State to manage video size
  const videoRef = useRef(null);

  const handleScroll = () => {
    if (videoRef.current) {
      const videoPosition = videoRef.current.getBoundingClientRect().top;
      const triggerPoint = window.innerHeight / 2; // Adjust trigger point as needed

      if (videoPosition <= triggerPoint && videoPosition > 0) {
        setVideoSize("big");
      } else if (videoPosition <= 0) {
        setVideoSize("small");
      }
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  const visibleCards = cardsData.slice(currentIndex, currentIndex + 5);

  return (
    <>
      <MyCarousel />
      <div className={`${styles.videoContainer} ${styles[videoSize]}`} ref={videoRef}>
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
        {visibleCards.map((card, index) => {
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
  position: fixed;
  left: -100%; /* Start off-screen to the left */
  top: 50%;
  transform: translateY(-50%);
  width: 60%;
  height: auto;
  transition: transform 0.6s ease, left 0.6s ease, width 0.6s ease, opacity 0.6s ease;
  opacity: 0;
}

.videoContainer.big {
  left: 50%;
  transform: translate(-50%, -50%); /* Center the video */
  width: 100%; /* Expand to full width */
  opacity: 1;
}

.videoContainer.small {
  left: -100%; /* Exit to the left */
  transform: translateY(-50%) scale(0.8); /* Shrink and move up */
  width: 60%;
  opacity: 0;
}

.video {
  width: 100%;
  height: auto;
  border-radius: 10px; /* Optional: add some border radius for smoother look */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Optional: shadow for depth */
}

.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap; /* Allow wrapping */
}

/* Other styles unchanged */
