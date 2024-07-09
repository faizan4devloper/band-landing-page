import React from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage"; // Import the new component
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
          <Route path="/all-cards" element={<AllCardsPage cardsData={cardsData} />} /> {/* Pass cardsData to AllCardsPage */}
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
        <div className={styles.viewAllContainer}>
          <h3 className={styles.solutionHead}>Solution Listing</h3>
          <Link to="/all-cards" className={styles.viewAllButton}>
            All Solution Catalog
          </Link>
        </div>
        <span
          className={`${styles.arrow} ${styles.leftArrow}`}
          onClick={handleClickLeft}
        >
          <FontAwesomeIcon icon={faArrowLeft} title="Previous"/>
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
          <FontAwesomeIcon icon={faArrowRight} title="Next"/>
        </span>
      </div>
    </>
  );
};

export default App;

import React from "react";
import styles from "./AllCardsPage.module.css"; // Create a new CSS module for styling
import Cards from "./Cards";
import { cardsData } from "../../data"; // Import cardsData from the data file

const AllCardsPage = ({ cardsData }) => {
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


.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  padding: 20px;
}
