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
    return (
      <li
        className={indicatorClasses}
        onClick={() => handleIndicatorClick(index)}
        tabIndex={0}
        key={index}
        style={{ background: index === selectedIndex ? '#6f36cd' : '#ccc' }}
      />
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



.carouselContainer {
  width: 100%;
  margin: 90px 0px 50px 0px;
}

.carouselItem {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  height: 280px;
  margin-bottom: 20px;
  overflow: hidden;
  width: 85%;
  border-radius: 10px;
  cursor: pointer;
}

.carouselImage {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.carouselImage6 {
  width: 100%;
  height: 100%;
  object-fit: fill;
  object-position: center;
}

.carouselOverlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 100%);
  border-radius: 6px;
}

.carouselCaption {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: white;
  font-family: "Poppins", sans-serif;
  z-index: 1;
}

.carouselCaption h2 {
  display: flex;
  font-weight: 600;
  font-size: 38px;
  margin: 0;
  padding: 15px;
  color: #fff;
}

.carouselCaption span {
  align-items: center;
  font-size: 16px;
  font-weight: 600;
  border-left: 3px solid rgba(255, 255, 255, 1);
  padding: 18px 10px;
  margin-left: 15px;
  color: #fff;
}

.carousel .slide {
  min-width: 100%;
  margin: 0;
  height: 352px !important;
  position: relative;
  text-align: center;
  overflow: hidden;
}

/* Custom Indicator Styles */
.indicatorsContainer {
  display: flex;
  justify-content: center;
  list-style: none;
  padding: 0;
  margin-top: 10px;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: inline-block;
  margin: 0 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.dot.selected {
  background-color: #6f36cd;
}
