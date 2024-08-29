
import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MyCarousel.module.css";

import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./Slide1.PNG";
import imgCarousel4 from "./Slide2.PNG";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./Slide3.PNG";

const MyCarousel = () => {
  const [isHovered, setIsHovered] = useState(false);
  const [selectedIndex, setSelectedIndex] = useState(0);
  
  const CarouselHead = "GenAI Deployable Assets";

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
    const tooltipText = "Our Hackathon Wins 🏆";

    return (
      <li
        className={`${indicatorClasses} ${index === 1 ? styles.tooltipContainer : ""}`}
        onClick={() => handleIndicatorClick(index)}
        tabIndex={0}
        key={index}
        style={{ background: index === selectedIndex ? '#6f36cd' : '#ccc' }}
      >
        {index === 1 && <span className={styles.tooltip}>{tooltipText}</span>}
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
            <h2>AWS<span>{CarouselHead}</span></h2>
          </div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel6} className={styles.carouselImage6} alt="Slide 2" />
          <div className={styles.carouselOverlay6}></div>
        </div>
        
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel3} className={styles.carouselImage6} alt="Slide 4" />
          <div className={styles.carouselOverlay6}></div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel4} className={styles.carouselImage6} alt="Slide 5" />
          <div className={styles.carouselOverlay6}></div>
          ]
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel5} className={styles.carouselImage} alt="Slide 6" />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2>AWS EBU<span>{CarouselHead}</span></h2>
          </div>
        </div>
      </Carousel>
      <ul className={styles.indicatorsContainer}>
        {[...Array(5)].map((_, index) => renderIndicator(index))}
      </ul>
    </div>
  );
};

export default MyCarousel;
