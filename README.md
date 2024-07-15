import React, { useState, useRef } from "react";
import styles from "./MyCarousel.module.css";

import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./carousel3.jpg";
import imgCarousel4 from "./carousel4.jpg";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./banner-1.png";

const MyCarousel = ({ isModalOpen }) => {
  const [activeIndex, setActiveIndex] = useState(0);
  const carouselRef = useRef(null);

  const handleSlideChange = (index) => {
    setActiveIndex(index);
    if (carouselRef.current) {
      carouselRef.current.goTo(index);
    }
  };

  const slides = [
    { image: imgCarousel, caption: "AWS Gen AI Pilots" },
    { image: imgCarousel6, caption: "AWS EBU" },
    { image: imgCarousel, caption: "AWS EBU Gen AI Pilots" },
    { image: imgCarousel3, caption: "AWS EBU Gen AI Pilots" },
    { image: imgCarousel4, caption: "AWS EBU Gen AI Pilots" },
    { image: imgCarousel5, caption: "AWS EBU Gen AI Pilots" },
  ];

  return (
    <div className={styles.carouselContainer}>
      <div className={styles.carousel}>
        <div className={styles.slides}>
          {slides.map((slide, index) => (
            <div className={`${styles.slide} ${index === activeIndex ? styles.active : ""}`} key={index}>
              <img src={slide.image} alt={`Slide ${index}`} className={styles.carouselImage} />
              <div className={styles.carouselOverlay}></div>
              <div className={styles.carouselCaption}>
                <h2>{slide.caption}</h2>
              </div>
            </div>
          ))}
        </div>
        <div className={styles.indicators}>
          {slides.map((slide, index) => (
            <div
              key={index}
              className={`${styles.dot} ${index === activeIndex ? styles.activeDot : ""}`}
              onClick={() => handleSlideChange(index)}
            />
          ))}
        </div>
      </div>
    </div>
  );
};

export default MyCarousel;


.carouselContainer {
  width: 100%;
  margin: 90px 0px 50px 0px;
}

.carousel {
  position: relative;
  overflow: hidden;
}

.slides {
  display: flex;
  transition: transform 0.5s ease;
}

.slide {
  width: 100%;
  position: relative;
}

.carouselImage {
  width: 100%;
  height: 100%;
  object-fit: cover;
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
  z-index: 1;
}

.carouselCaption {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
  color: white;
  z-index: 2;
}

.indicators {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 2;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background-color: #ccc;
  margin: 0 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.dot:hover {
  background-color: #6f36cd;
}

.activeDot {
  background-color: #6f36cd;
}

.active {
  opacity: 1;
  z-index: 1;
}
