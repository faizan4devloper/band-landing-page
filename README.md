import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
// import Footer from "./components/Footer/Footer";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/AllCardsPage"; // Import the new component
import { cardsData } from "./data"; // Import card data from a separate file

const App = () => {
  const [bigIndex, setBigIndex] = React.useState(null);

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
              />
            }
          />
          <Route path="/dashboard" element={<SideBarPage />} />
          <Route path="/all-cards" element={<AllCardsPage />} /> {/* Add the new route */}
        </Routes>
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
}) => {
  return (
    <>
      <MyCarousel />
      <div className={styles.cardsContainer}>
        <span
          className={`${styles.arrow} ${styles.leftArrow}`}
          onClick={handleClickLeft}
        >
          <FontAwesomeIcon icon={faArrowLeft} title="Previous"/>
        </span>
        {cardsData.slice(0, 5).map((card, index) => (  // Display only the first 5 cards
          <Cards
            key={index}
            imageUrl={card.imageUrl}
            title={card.title}
            description={card.description}
            isBig={index === bigIndex}
            toggleSize={() => toggleSize(index)}
            onClick={Cards}
          />
        ))}
        <span
          className={`${styles.arrow} ${styles.rightArrow}`}
          onClick={handleClickRight}
        >
          <FontAwesomeIcon icon={faArrowRight} title="Next"/>
        </span>
      </div>
      <div className={styles.viewAllContainer}>
        <Link to="/all-cards" className={styles.viewAllButton}>
          View All
        </Link>
      </div>
    </>
  );
};

export default App;



// components/AllCardsPage.js
import React from "react";
import styles from "./AllCardsPage.module.css"; // Create a new CSS module for styling
import Cards from "./Cards/Cards";
import { cardsData } from "../data";

const AllCardsPage = () => {
  return (
    <div className={styles.allCardsContainer}>
      {cardsData.map((card, index) => (
        <Cards
          key={index}
          imageUrl={card.imageUrl}
          title={card.title}
          description={card.description}
          isBig={false} // Ensure all cards are in the default small size
        />
      ))}
    </div>
  );
};

export default AllCardsPage;


/* components/AllCardsPage.module.css */
.allCardsContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 20px;
}

.viewAllContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.viewAllButton {
  padding: 10px 20px;
  background-color: #5f1ec1;
  color: white;
  text-decoration: none;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.viewAllButton:hover {
  background-color: #4b17a2;
}
