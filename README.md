import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MyCarousel.module.css";

import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./Slide1.JPG";
import imgCarousel4 from "./Slide2.JPG";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./Slide3.JPG";

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
        {/* Slide 1 */}
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel} className={styles.carouselImage} alt="Slide 1" />
          <div className={styles.carouselOverlay1}></div>
          <div className={styles.carouselCaption}>
            <h2>AWS<span>{CarouselHead}</span></h2>
          </div>
        </div>
        
        {/* Slide 2 */}
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel6} className={styles.carouselImage} alt="Slide 2" style={{ objectFit: "contain" }} />
          <div className={styles.carouselOverlay2}></div>
        </div>
        
        {/* Slide 3 */}
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel3} className={styles.carouselImage} alt="Slide 3" />
          <div className={styles.carouselOverlay3}></div>
        </div>
        
        {/* Slide 4 */}
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel4} className={styles.carouselImage} alt="Slide 4" />
          <div className={styles.carouselOverlay4}></div>
        </div>
        
        {/* Slide 5 */}
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel5} className={styles.carouselImage} alt="Slide 5" />
          <div className={styles.carouselOverlay5}></div>
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





.carouselContainer {
  width: 100%;
  margin: 90px 0px 50px 0px;
}

.carouselItem {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  height: 320px; /* Adjusted height */
  margin-bottom: 20px;
  overflow: hidden;
  width: 90%; /* Adjusted width */
  border-radius: 10px;
  cursor: pointer;
}

.carouselImage {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Adjust to fit your needs */
  object-position: center;
}

.carouselOverlay1 {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 100%);
  border-radius: 6px;
}

.carouselOverlay2 {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #123456 0%, rgba(31, 119, 246, 0.73) 100%);
  border-radius: 6px;
}

.carouselOverlay3 {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #abcdef 0%, rgba(31, 119, 246, 0.73) 100%);
  border-radius: 6px;
}

.carouselOverlay4 {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #ff9933 0%, rgba(0, 0, 0, 0.5) 100%);
  border-radius: 6px;
}

.carouselOverlay5 {
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
  width: 22px;
  height: 3px;
  display: inline-block;
  margin: 0 5px;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1), 0 4px 8px rgba(0,0,0,0.15);
}

.dot.selected {
  background: #6f36cd !important;
}

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
  font-size: 8px;
  z-index: 1;
  top: 300%;
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
  bottom: 80%;
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: #6f36cd transparent transparent transparent;
}









import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MyCarousel.module.css";

import imgCarousel from "./carousel1.jpg";
import imgCarousel3 from "./Slide1.JPG";
import imgCarousel4 from "./Slide2.JPG";
import imgCarousel5 from "./carousel5.jpg";
import imgCarousel6 from "./Slide3.JPG";

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
          <img src={imgCarousel3} className={styles.carouselImage} alt="Slide 4" />
          <div className={styles.carouselOverlay6}></div>
        </div>
        <div className={styles.carouselItem} onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
          <img src={imgCarousel4} className={styles.carouselImage} alt="Slide 5" />
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


/* Custom Indicator Styles */
.indicatorsContainer {
  display: flex;
  justify-content: center;
  list-style: none;
  padding: 0;
  margin-top: 10px;
}

.dot {
  width: 22px; /* 10px*/
  height: 3px;
  /*border-radius: 50%;*/
  display: inline-block;
  margin: 0 5px;
  cursor: pointer;
box-shadow: 0 2px 4px rgba(0,0,0,0.1), 0 4px 8px rgba(0,0,0,0.15);
 /*box-shadow: 0 2px 4px rgba(0,0,0,0.2), 0 3px 6px rgba(0,0,0,0.2); */
 
 
  
}

.dot.selected {
  background: #6f36cd !important; /* Purple color for selected dot */
}


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
  font-size: 8px;
  z-index: 1;
  top: 300%; /* Position above the dot */
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
  bottom: 80%; /* At the bottom of the tooltip */
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: #6f36cd transparent transparent transparent;
}
