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
          <div className={styles.carouselOverlay6}></div>
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
/*.carouselOverlay6 {*/
/*  position: absolute;*/
/*  top: 0;*/
/*  left: 0;*/
/*  width: 100%;*/
/*  height: 100%;*/
/*  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 100%);*/
/*  border-radius: 6px;*/
/*}*/

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

.carousel .control-dots {
  background-color: rgba(95, 30, 193, 1) !important;
  z-index: 0 !important;
  
}

.carouselContainer .carousel .control-dots .dot.selected {
  background-color: #6f36cd !important;
  
}

/* Custom Dot Indicator Styles */
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
  /*box-shadow: 0px 0px 10px #6f36cd;*/
}
