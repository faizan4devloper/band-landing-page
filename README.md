import React, { useState } from "react";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";

const AllCardsPage = ({ cardsData, cardsContainerRef }) => {
  const [bigIndex, setBigIndex] = useState(null);

  const handleBackButtonClick = () => {
    window.history.back();
  };

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  return (
    <div className={styles.allCardsPage}>
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <div className={styles.allCardsContainer} ref={cardsContainerRef}>
        {cardsData.map((card, index) => (
          <Cards
            key={index}
            imageUrl={card.imageUrl}
            title={card.title}
            description={card.description}
            isBig={index === bigIndex}
            toggleSize={() => toggleSize(index)}
          />
        ))}
      </div>
    </div>
  );
};

export default AllCardsPage;




import React, { useRef } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage"; // Import the new component
import { cardsData } from "./data"; // Import card data from a separate file

const App = () => {
  const [bigIndex, setBigIndex] = React.useState(null);
  const cardsContainerRef = useRef(null);

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

  const handleScrollDown = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: "smooth" });
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
                bigIndex={bigIndex}
                toggleSize={toggleSize}
                cardsContainerRef={cardsContainerRef}
              />
            }
          />
          <Route path="/dashboard" element={<SideBarPage />} />
          <Route
            path="/all-cards"
            element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
          />
        </Routes>
        <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Go Down">
          <FontAwesomeIcon icon={faChevronDown} />
        </div>
      </div>
    </Router>
  );
};

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  bigIndex,
  toggleSize,
  cardsContainerRef,
}) => {
  return (
    <>
      <MyCarousel />
      <div className={styles.cardsContainer} ref={cardsContainerRef}>
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
          </Link>
        </div>
        <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
          <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
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
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
};

export default App;



import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize }) => {
  const [isHovered, setIsHovered] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h2>{title}</h2>
            <p>{description}</p>
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}`,
              }}
              className={styles.readMoreLink}
            >
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${
                  isHovered ? styles.hovered : ""
                }`}
                onMouseEnter={() => setIsHovered(true)}
                onMouseLeave={() => setIsHovered(false)}
              >
                <span
                  style={{
                    fontSize: isHovered ? "0.8em" : "1em",
                  }}
                >
                  {isHovered && "Read More "}
                </span>
                <span
                  style={{
                    marginLeft: isHovered ? "5px" : "0",
                    fontSize: isHovered ? "1.3em" : "1em",
                  }}
                >
                  <FontAwesomeIcon icon={faArrowRight} />
                </span>
              </span>
            </Link>
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;
