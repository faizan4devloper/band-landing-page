import React, { useState, useEffect, useRef } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Card from "./components/Cards/Card";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage";
import { cardsData } from "./data";

const App = () => {
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [cards, setCards] = useState(cardsData); // state to manage cards
  const cardsContainerRef = useRef(null);

  useEffect(() => {
    // Set the last card as active when a new card is added
    setBigIndex(cards.length - 1);
  }, [cards]);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    setCurrentIndex(currentIndex === 0 ? cards.length - 5 : currentIndex - 1);
  };

  const handleClickRight = () => {
    setCurrentIndex(currentIndex === cards.length - 5 ? 0 : currentIndex + 1);
  };

  const handleScrollDown = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: "smooth" });
      setShowScrollDown(false);
      setShowScrollUp(true);
    }
  };

  const handleScrollUp = () => {
    window.scrollTo({ top: 0, behavior: "smooth" });
    setShowScrollDown(true);
    setShowScrollUp(false);
  };

  const handleMouseEnter = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: "smooth", block: "start" });
    }
  };

  useEffect(() => {
    const handleScroll = () => {
      if (window.scrollY > 50) {
        setShowScrollUp(true);
        setShowScrollDown(false);
      } else {
        setShowScrollUp(false);
        setShowScrollDown(true);
      }
    };

    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  return (
    <Router>
      <div className={styles.app}>
        <Header />
        <MyCarousel />
        <div className={styles.cardsContainer} ref={cardsContainerRef}>
          {cards.slice(currentIndex, currentIndex + 5).map((card, index) => (
            <Card
              key={index}
              imageUrl={card.imageUrl}
              title={card.title}
              description={card.description}
              isBig={bigIndex === currentIndex + index}
              toggleSize={() => toggleSize(currentIndex + index)}
              isNew={bigIndex === currentIndex + index && cards.length - 1 === currentIndex + index}
            />
          ))}
        </div>
        <div className={styles.navigation}>
          <button onClick={handleClickLeft}>
            <FontAwesomeIcon icon={faArrowLeft} />
          </button>
          <button onClick={handleClickRight}>
            <FontAwesomeIcon icon={faArrowRight} />
          </button>
        </div>
        {showScrollDown && (
          <button onClick={handleScrollDown} className={styles.scrollButton}>
            <FontAwesomeIcon icon={faChevronDown} />
          </button>
        )}
        {showScrollUp && (
          <button onClick={handleScrollUp} className={styles.scrollButton}>
            <FontAwesomeIcon icon={faChevronUp} />
          </button>
        )}
        <Routes>
          <Route path="/dashboard" element={<SideBarPage />} />
          <Route path="/all-cards" element={<AllCardsPage />} />
        </Routes>
      </div>
    </Router>
  );
};

export default App;
