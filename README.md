import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MyCarousel.module.css";

import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./carousel3.jpg";
import imgCarousel4 from "./carousel4.jpg";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./banner-1.png";

const MyCarousel = ({ isModalOpen }) => {
  const [isHovered, setIsHovered] = useState(false);

  const handleMouseEnter = () => {
    setIsHovered(true);
  };

  const handleMouseLeave = () => {
    setIsHovered(false);
  };

  // Custom renderIndicator function for animated colorful dots
  const renderIndicator = (onClickHandler, isSelected, index, label) => {
    const indicatorClasses = `${styles.dot} ${isSelected ? styles.selected : ""}`;
    return (
      <li
        className={indicatorClasses}
        onClick={onClickHandler}
        onKeyDown={onClickHandler}
        tabIndex={0}
        title={`${label} ${index + 1}`}
        aria-label={`${label} ${index + 1}`}
        key={index}
        style={{ background: isSelected ? '#6f36cd' : '#ccc' }}
      />
    );
  };

  // Conditionally render the carousel based on modal state
  if (isModalOpen) {
    return null; // Return null or an empty div when modal is open
  }

  return (
    <div className={styles.carouselContainer}>
      <Carousel
        showArrows={false}
        showThumbs={false}
        showIndicators={true}
        infiniteLoop={true}
        autoPlay={!isHovered}
        showStatus={false}
        interval={2000}
        stopOnHover={false}
        renderIndicator={renderIndicator}
        className={styles.customIndicator}
      >
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2>AWS<span>Gen AI Pilots</span></h2>
          </div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel6} className={styles.carouselImage6} />
          <div className={styles.carouselOverlay}></div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>AWS EBU</h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel3} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>AWS EBU</h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel4} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>AWS EBU</h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel5} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2>AWS EBU<span>Gen AI Pilots</span></h2>
          </div>
        </div>
      </Carousel>
    </div>
  );
};

export default MyCarousel;
