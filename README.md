import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MyCarousel.module.css";

import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./carousel3.jpg";
import imgCarousel4 from "./carousel4.jpg";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./banner-1.png";

const MyCarousel = () => {
  const [isHovered, setIsHovered] = useState(false);
  const [selectedIndex, setSelectedIndex] = useState(0);

  const handleMouseEnter = () => {
    setIsHovered(true);
  };

  const handleMouseLeave = () => {
    setIsHovered(false);
  };

  const handleIndicatorClick = (index) => {
    setSelectedIndex(index);
  };

  const renderIndicator = (index) => {
    const indicatorClasses = `${styles.dot} ${index === selectedIndex ? styles.selected : ""}`;
    const tooltips = [
      "Tooltip for slide 1",
      "Tooltip for slide 2",
      "Tooltip for slide 3",
      "Tooltip for slide 4",
      "Tooltip for slide 5",
      "Tooltip for slide 6"
    ];

    return (
      <li
        className={`${indicatorClasses} ${styles.tooltipContainer}`}
        onClick={() => handleIndicatorClick(index)}
        tabIndex={0}
        key={index}
        style={{ background: index === selectedIndex ? '#6f36cd' : '#ccc' }}
      >
        <span className={styles.tooltip}>{tooltips[index]}</span>
      </li>
    );
  };

  return (
    <div className={styles.carouselContainer}>
      <Carousel
        selectedItem={selectedIndex}
        onChange={handleIndicatorClick}
        showArrows={false}
        showThumbs={false}
        showIndicators={false}
        infiniteLoop={true}
        autoPlay={!isHovered}
        showStatus={false}
        interval={2000}
        stopOnHover={false}
        className={styles.customIndicator}
      >
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel} className={styles.carouselImage} alt="Slide 1" />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2>AWS<span>Gen AI Pilots</span></h2>
          </div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel6} className={styles.carouselImage6} alt="Slide 2" />
          <div className={styles.carouselOverlay6}></div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel} className={styles.carouselImage} alt="Slide 3" />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>AWS EBU</h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel3} className={styles.carouselImage} alt="Slide 4" />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>AWS EBU</h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel4} className={styles.carouselImage} alt="Slide 5" />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>AWS EBU</h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel5} className={styles.carouselImage} alt="Slide 6" />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2>AWS EBU<span>Gen AI Pilots</span></h2>
          </div>
        </div>
      </Carousel>
      <ul className={styles.indicatorsContainer}>
        {[...Array(6)].map((_, index) => renderIndicator(index))}
      </ul>
    </div>
  );
};

export default MyCarousel;


/* MyCarousel.module.css */
.tooltipContainer {
  position: relative;
  display: inline-block;
  cursor: pointer;
}

.tooltip {
  visibility: hidden;
  width: 120px;
  background-color: #6f36cd;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;
  position: absolute;
  z-index: 1;
  bottom: 125%; /* Position above the dot */
  left: 50%;
  margin-left: -60px;
  opacity: 0;
  transition: opacity 0.3s;
}

.tooltipContainer:hover .tooltip {
  visibility: visible;
  opacity: 1;
}

.tooltip::after {
  content: "";
  position: absolute;
  top: 100%; /* At the bottom of the tooltip */
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: #6f36cd transparent transparent transparent;
}
