import React, { useState, useEffect, useRef } from "react";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";

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

      if (videoPosition <= triggerPoint) {
        setVideoSize("big");
      } else {
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
          src="path/to/your/video.mp4"
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
  width: 100%;
  display: flex;
  justify-content: center;
  margin: 20px 0; /* Space around the video */
  transition: transform 0.6s ease, width 0.6s ease, height 0.6s ease;
}

.videoContainer.small {
  transform: scale(0.8); /* Small state */
  width: 60%; /* Adjust width for small state */
  height: auto; /* Maintain aspect ratio */
}

.videoContainer.big {
  transform: scale(1.2); /* Big state */
  width: 100%; /* Full width when big */
  height: auto; /* Maintain aspect ratio */
}

.video {
  width: 100%;
  height: 100%;
  border-radius: 12px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
}
