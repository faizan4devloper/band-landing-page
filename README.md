import React from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MyCarousel.module.css"; // Import CSS module

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
        className={`${styles.customCarousel} ${styles.customIndicator}`} // Apply the customIndicator class
        renderIndicator={(onClickHandler, isSelected, index, label) => {
          const defStyle = {
            marginLeft: 20,
            color: "white",
            cursor: "pointer",
            zIndex: 0, // Add z-index style here
          };
          const style = isSelected
            ? { ...defStyle, color: "red" }
            : { ...defStyle };
          return (
            <li
              style={style}
              onClick={onClickHandler}
              onKeyDown={onClickHandler}
              value={index}
              key={index}
              role="button"
              tabIndex={0}
              aria-label={`${label} ${index + 1}`}
            >
              •
            </li>
          );
        }}
      >
        <div
          className={styles.carouselItem}
          style={{
            background:
              "linear-gradient(90deg, #6F36CD 0%, rgba(31, 119, 246, 0.27) 100%)",
          }}
        >
          <img src={imgCarousel} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2>
              AWS<span>Gen AI Pilots</span>
            </h2>
          </div>
        </div>
        <div className={styles.carouselItem}>
          <img src={imgCarousel6} className={styles.carouselImage6} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
        </div>

        <div className={styles.carouselItem}>
          <img src={imgCarousel} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>

        <div className={styles.carouselItem}>
          <img src={imgCarousel3} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem}>
          <img src={imgCarousel4} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem}>
          <img src={imgCarousel5} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div> {/* Add overlay */}
          <div className={styles.carouselCaption}>
            <h2>
              AWS EBU<span>Gen AI Pilots</span>
            </h2>
          </div>
        </div>
      </Carousel>
    </div>
  );
};

export default MyCarousel;
