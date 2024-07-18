  
  import React, { useRef, useState, useEffect } from "react";
  import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
  import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
  import { faArrowRight, faArrowLeft, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
  import Header from "./components/Header/Header";
  import MyCarousel from "./components/Carousel/MyCarousel";
  import Cards from "./components/Cards/Cards";
  import styles from "./App.module.css";
  import SideBarPage from "./components/Sidebar/SideBarPage";
  import AllCardsPage from "./components/Cards/AllCardsPage";
  import { cardsData as initialCardsData } from "./data";
  
  const App = () => {
    const [cardsData, setCardsData] = useState(initialCardsData);
    const [currentIndex, setCurrentIndex] = useState(0);
    const [bigIndex, setBigIndex] = useState(0);
    const [showScrollDown, setShowScrollDown] = useState(true);
    const [showScrollUp, setShowScrollUp] = useState(false);
    const cardsContainerRef = useRef(null);
    
  
    const toggleSize = (index) => {
      setBigIndex(index === bigIndex ? null : index);
    };
  
    const handleClickLeft = () => {
      const newBigIndex = bigIndex === 0 ? cardsData.length - 1 : bigIndex - 1;
      setBigIndex(newBigIndex);
  
      const newCurrentIndex = newBigIndex < currentIndex ? newBigIndex : currentIndex;
      if(newCurrentIndex < currentIndex && currentIndex !== 0){
      setCurrentIndex(newCurrentIndex);
      }else{
        setCurrentIndex(newCurrentIndex)
      }
    };
  
    const handleClickRight = () => {
      const newBigIndex = bigIndex === cardsData.length - 1 ? 0 : bigIndex + 1;
      setBigIndex(newBigIndex);
  
      const newCurrentIndex = newBigIndex > currentIndex + 4 ? newBigIndex - 4 : currentIndex;
      setCurrentIndex(newCurrentIndex);
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
  
      const debounceScroll = debounce(handleScroll, 100);
      window.addEventListener("scroll", debounceScroll);
  
      return () => window.removeEventListener("scroll", debounceScroll);
    }, []);
  
    const debounce = (func, wait) => {
      let timeout;
      return function executedFunction(...args) {
        const later = () => {
          clearTimeout(timeout);
          func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
      };
    };
  
    const addNewCard = () => {
      const newCard = {
        imageUrl: "path/to/new/image.jpg",
        title: "New Card Title",
        description: "New Card Description",
      };
      const newCardsData = [...cardsData, newCard];
      setCardsData(newCardsData);
  
      const newCardIndex = newCardsData.length - 1;
      setBigIndex(newCardIndex);
  
      if (newCardIndex > currentIndex + 4) {
        setCurrentIndex(newCardIndex - 4);
      } else {
        setCurrentIndex(currentIndex);
      }
    };
  
    return (
      <Router>
        <div className={styles.app}>
          <Header />
          <Routes>
            <Route
              path="/"
              element={
                <Home
                  cardsData={cardsData}
                  handleClickLeft={handleClickLeft}
                  handleClickRight={handleClickRight}
                  currentIndex={currentIndex}
                  bigIndex={bigIndex}
                  toggleSize={toggleSize}
                  cardsContainerRef={cardsContainerRef}
                  handleMouseEnter={handleMouseEnter}
                />
              }
            />
            <Route path="/dashboard" element={<SideBarPage />} />
            <Route
              path="/all-cards"
              element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
            />
          </Routes>
          {showScrollDown && (
            <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
              <FontAwesomeIcon icon={faChevronDown} />
            </div>
          )}
          {showScrollUp && (
            <div className={styles.scrollUpButton} onClick={handleScrollUp} title="Scroll Up">
              <FontAwesomeIcon icon={faChevronUp} />
            </div>
          )}
        </div>
      </Router>
    );
  };
  
  const Home = ({
    cardsData,
    handleClickLeft,
    handleClickRight,
    currentIndex,
    bigIndex,
    toggleSize,
    cardsContainerRef,
    handleMouseEnter,
  }) => {
    const visibleCards = cardsData.slice(currentIndex, currentIndex + 5);
    return (
      <>
        <MyCarousel />
        <div
          className={styles.cardsContainer}
          ref={cardsContainerRef}
          onMouseEnter={handleMouseEnter}
        >
          <div className={styles.viewAllContainer}>
            <Link to="/all-cards" className={styles.viewAllButton}>
              View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
            </Link>
          </div>
          <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
            <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
          </span>
          {visibleCards.map((card, index) => {
            const actualIndex = currentIndex + index;
            return (
              <Cards
                key={index}
                imageUrl={card.imageUrl}
                title={card.title}
                description={card.description}
                isBig={actualIndex === bigIndex}
                toggleSize={() => toggleSize(actualIndex)}
              />
            );
          })}
          <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
            <FontAwesomeIcon icon={faArrowRight} title="Next" />
          </span>
        </div>
      </>
    );
  };
  
  export default App;



import React, { useState, useRef } from "react";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards";
import CategorySidebar from "./CategorySidebar";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";

import BusinessSVG from "./business.svg";
import IndustrySVG from "./industry.svg";

const AllCardsPage = ({ cardsData }) => {
  const [bigIndex, setBigIndex] = useState(null);
  const [filteredCards, setFilteredCards] = useState(cardsData);
  const cardsContainerRef = useRef(null);

  const handleBackButtonClick = () => {
    window.history.back();
  };

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleFilterChange = (category, item) => {
    const filtered = cardsData.filter(card => card.category === category && card.type === item);
    setFilteredCards(filtered);
  };

  const categories = [
    {
      name: "Industry",
      svgIcon: IndustrySVG,
      items: ["LSH", "BFSI", "ENU", "Automative", "GOVT"]
    },
    {
      name: "Business Function",
      svgIcon: BusinessSVG,
      items: ["SDLC", "HR", "Customer Support", "Finance"]
    },
    // Add other categories as needed
  ];

  return (
    <div className={styles.allCardsPage}>
      <CategorySidebar categories={categories} onFilterChange={handleFilterChange} />
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h4 className={styles.catalogsHeading}>All Solutions Catalog</h4>
      <div className={styles.mainContainerCards}>
        <div className={styles.allCardsContainer} ref={cardsContainerRef}>
          {filteredCards.map((card, index) => (
            <div key={index}>
              <Cards
                imageUrl={card.imageUrl}
                title={card.title}
                description={card.description}
                isBig={index === bigIndex}
                toggleSize={() => toggleSize(index)}
              />
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};

export default AllCardsPage;
