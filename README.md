import React from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MyCarousel.module.css";
import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./carousel3.jpg";
import imgCarousel4 from "./carousel4.jpg";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./banner-1.png";

const MyCarousel = ({ isModalOpen }) => {
  return (
    <div className={styles.carouselContainer}>
      <Carousel
        showArrows={false}
        showThumbs={false}
        showIndicators={true}
        infiniteLoop={true}
        autoPlay={true}
        interval={2000}
        stopOnHover={true}
        className={styles.customIndicator}
      >
        <div className={styles.carouselItem} style={{ background: "linear-gradient(90deg, #6F36CD 0%, rgba(31, 119, 246, 0.27) 100%)" }}>
          <img src={imgCarousel} alt="Carousel Image" className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2>AWS<span>Gen AI Pilots</span></h2>
          </div>
        </div>

        <div className={styles.carouselItem}>
          <img src={imgCarousel6} alt="Carousel Image" className={styles.carouselImage6} />
          <div className={styles.carouselOverlay6}></div>
        </div>

        <div className={styles.carouselItem}>
          <img src={imgCarousel} alt="Carousel Image" className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>AWS EBU</h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>

        <div className={styles.carouselItem}>
          <img src={imgCarousel3} alt="Carousel Image" className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>AWS EBU</h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>

        <div className={styles.carouselItem}>
          <img src={imgCarousel4} alt="Carousel Image" className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>AWS EBU</h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>

        <div className={styles.carouselItem}>
          <img src={imgCarousel5} alt="Carousel Image" className={styles.carouselImage} />
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
  margin-top: 50px;
}

.carouselItem {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  height: 280px; /* Adjust the height as per your design */
  margin: 70px 0;
  overflow: hidden;
  width: 85%;
  border-radius: 10px;
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
  border-radius: 6px;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    #6f36cd 0%,
    rgba(31, 119, 246, 0.73) 100%
  );
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
}

.carouselCaption span {
  align-items: center;
  font-size: 16px;
  font-weight: 600;
  border-left: 3px solid rgba(255, 255, 255, 1);
  padding: 18px 10px;
  margin-left: 15px;
}

.carousel .slide {
  min-width: 100%;
  margin: 0;
  height: 352px !important; /* Adjust the slide height as needed */
  position: relative;
  text-align: center;
  overflow: hidden; /* Ensure contents stay within the defined height */
}

.carousel .control-dots {
  background-color: rgba(95, 30, 193, 1) !important;
  z-index: 0 !important;
}

.carouselContainer .carousel .control-dots .dot.selected {
  background-color: #6f36cd !important;
}
