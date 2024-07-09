import React, { useEffect, useState } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowCircleUp, faArrowCircleLeft, faArrowCircleRight } from "@fortawesome/free-solid-svg-icons"; // Import FontAwesome icons
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import AllCardsPage from "./components/Cards/AllCardsPage"; // Import the new component
import { cardsData } from "./data"; // Import card data from a separate file

const App = () => {
  const [bigIndex, setBigIndex] = React.useState(null);
  const [showScrollButton, setShowScrollButton] = useState(false);
  const [lastScrollTop, setLastScrollTop] = useState(0);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    if (bigIndex === null || bigIndex === 0) {
      setBigIndex(cardsData.length - 1);
    } else {
      setBigIndex(bigIndex - 1);
    }
  };

  const handleClickRight = () => {
    if (bigIndex === null || bigIndex === cardsData.length - 1) {
      setBigIndex(0);
    } else {
      setBigIndex(bigIndex + 1);
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  const handleScroll = () => {
    const currentScrollTop = window.pageYOffset || document.documentElement.scrollTop;
    setShowScrollButton(currentScrollTop > lastScrollTop && currentScrollTop > 100);
    setLastScrollTop(currentScrollTop <= 0 ? 0 : currentScrollTop);
  };

  const scrollToTop = () => {
    window.scrollTo({
      top: 0,
      behavior: "smooth"
    });
  };

  return (
    <Router>
      <div className={styles.app}>
        <Header />
        <MyCarousel />
        <div className={styles.cardsContainer}>
          <div className={styles.viewAllContainer}>
            <Link to="/all-cards" className={styles.viewAllButton}>
              View All Solutions
            </Link>
          </div>
          <span
            className={`${styles.arrow} ${styles.leftArrow}`}
            onClick={handleClickLeft}
          >
            <FontAwesomeIcon icon={faArrowCircleLeft} title="Previous"/>
          </span>
          {cardsData.slice(0, 5).map((card, index) => (
            <Cards
              key={index}
              imageUrl={card.imageUrl}
              title={card.title}
              description={card.description}
              isBig={index === bigIndex}
              toggleSize={() => toggleSize(index)}
            />
          ))}
          <span
            className={`${styles.arrow} ${styles.rightArrow}`}
            onClick={handleClickRight}
          >
            <FontAwesomeIcon icon={faArrowCircleRight} title="Next"/>
          </span>
        </div>
        <Routes>
          <Route
            path="/all-cards"
            element={<AllCardsPage cardsData={cardsData} />}
          />
          {/* Add more routes as needed */}
        </Routes>
        <button
          className={`${styles.scrollButton} ${showScrollButton ? styles.show : ""}`}
          onClick={scrollToTop}
        >
          <FontAwesomeIcon icon={faArrowCircleUp} />
        </button>
      </div>
    </Router>
  );
};

export default App;
