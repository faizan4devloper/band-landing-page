import React, { useState } from "react";
import MyCarousel from "./MyCarousel";
import RequestDemoForm from "./RequestDemoForm";
import styles from "./ParentComponent.module.css"; // Assuming you have a CSS module for the parent component

const ParentComponent = () => {
  const [isFormVisible, setIsFormVisible] = useState(false);

  const openForm = () => {
    setIsFormVisible(true);
  };

  const closeForm = () => {
    setIsFormVisible(false);
  };

  return (
    <div>
      <button onClick={openForm}>Request a Demo</button>
      <div className={isFormVisible ? styles.hideDots : ""}>
        <MyCarousel />
      </div>
      {isFormVisible && <RequestDemoForm closeModal={closeForm} />}
    </div>
  );
};

export default ParentComponent;

import React, { useEffect } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MyCarousel.module.css";

import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./carousel3.jpg";
import imgCarousel4 from "./carousel4.jpg";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./banner-1.png";

const MyCarousel = () => {
  useEffect(() => {
    const style = document.createElement("style");
    style.innerHTML = `
      .carousel .control-dots {
        z-index: 10 !important;
      }
      .${styles.carouselOverlay}, .${styles.carouselOverlay6} {
        z-index: 1 !important;
      }
    `;
    document.head.appendChild(style);
    return () => {
      document.head.removeChild(style);
    };
  }, []);

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
      >
        <div
          className={styles.carouselItem}
          style={{
            background:
              "linear-gradient(90deg, #6F36CD 0%, rgba(31, 119, 246, 0.27) 100%)",
          }}
        >
          <img src={imgCarousel} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2>
              AWS<span>Gen AI Pilots</span>
            </h2>
          </div>
        </div>
        <div className={styles.carouselItem}>
          <img src={imgCarousel6} className={styles.carouselImage6} />
          <div className={styles.carouselOverlay6}></div>
        </div>

        <div className={styles.carouselItem}>
          <img src={imgCarousel} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        
        <div className={styles.carouselItem}>
          <img src={imgCarousel3} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem}>
          <img src={imgCarousel4} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
          <div className={styles.carouselCaption}>
            <h2 style={{ borderBottom: "3px solid rgba(255, 255, 255, 1)" }}>
              AWS EBU
            </h2>
            <p>Gen AI Pilots</p>
          </div>
        </div>
        <div className={styles.carouselItem}>
          <img src={imgCarousel5} className={styles.carouselImage} />
          <div className={styles.carouselOverlay}></div>
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



.hideDots .carousel .control-dots {
  display: none !important;
}
