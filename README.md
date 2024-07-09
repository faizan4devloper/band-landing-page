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

        <div className={styles.viewAllContainer}>
              <h3 className={styles.solutionHead}>Solution Listing</h3>
        <Link to="/all-cards" className={styles.viewAllButton}>
          View All
        </Link>
      </div>
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
      
    </>
  );
};

export default App;

// components/Cards/Cards.js
import React, { useState } from "react";
import { Link } from "react-router-dom"; // Import Link component
import styles from "./Cards.module.css"; // Import CSS module for card styles
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize }) => {
  const [isHovered, setIsHovered] = useState(false);

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
            {/* Add Link component to navigate to dashboard with title query parameter */}
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}`,
              }}
              className={styles.readMoreLink}
            >
              {/* Content of the "Read More" link */}
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${
                  isHovered ? styles.hovered : ""
                }`}
                onMouseEnter={() => setIsHovered(true)}
                onMouseLeave={() => setIsHovered(false)}
              >
                <span
                  style={{
                    fontSize: isHovered ? "0.8em" : "1em", // Decrease font size on hover
                  }}
                >
                  {isHovered && "Read More "}
                </span>
                <span
                  style={{
                    marginLeft: isHovered ? "5px" : "0", // Add space on hover
                    fontSize: isHovered ? "1.3em" : "1em", // Increase size on hover
                  }}
                >
<FontAwesomeIcon icon={faArrowRight} />                </span>
              </span>
            </Link>
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;

// components/Cards/AllCardsPage.js
import React from "react";
import styles from "./AllCardsPage.module.css"; // Create a new CSS module for styling
import Cards from "./Cards";
import { cardsData } from "../../data"; // Import cardsData from the data file

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

// components/Cards/AllCardsPage.module.css
.allCardsContainer {
  display: flex;
  margin-top: 50px;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  padding: 20px;
}

// App.module.css
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

@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");

/* Hide the scrollbar */
::-webkit-scrollbar {
  width: 0; /* Remove scrollbar width */
  height: 0; /* Remove scrollbar height */
}

html,
body {
  overflow-y: scroll; /* Always show vertical scrollbar */
  font-family: "Poppins", sans-serif;
}

.app {
  width: 1100px;
  height: 600px;
  margin: 0 auto;
}

.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
}

.arrow {
  cursor: pointer;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  font-size: 18px;
  width: 17px;
  height: 22px;
  padding: 3px 8px 5px 6px;
  border-radius: 50px;
  border: 2px solid rgba(15, 95, 220, 1);
  color: rgba(15, 95, 220, 1); /* Adjust color as needed */
  transition: transform 0.5s ease, background 0.5s ease; /* Add transition */
}

.arrow:hover {
  background-color: rgba(15, 95, 220, 1); /* Change background color on hover */
  color: white; /* Change text color on hover */
}

.leftArrow {
  left: -25px;
  top: 90px;
}

.rightArrow {
  right: -25px;
  top: 90px;
}

@media screen and (max-width: 1100px) {
  .app {
    padding: 0 10px;
  }
}

@media screen and (max-width: 768px) {
  .app {
    max-width: 100%;
  }
}

.arrow img::after {
  content: attr(title);
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: #fff;
  padding: 4px;
  border-radius: 5px;
  font-size: 8px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  /*transition: opacity 0.1s, visibility 0.1s;*/
}

.arrow img:hover::after {
  opacity: 1;
  visibility: visible;
}

.solutionHead {
  margin-top: 10px;
  text-align: center;
  margin-left: 3px;
  font-size: 20px;
  font-weight: 500;
  color: rgba(15, 95, 220, 1);
}

.leftArrow:hover {
  transform: translateY(-50%) scale(1.2); /* Add scaling effect */
}

.rightArrow:hover {
  transform: translateY(-50%) scale(1.2); /* Add scaling effect */
}

// components/Cards/Cards.module.css
.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  position: relative;
}

.card {
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  width: 200px;
  height: 300px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 12px;
  cursor: pointer;
  transition: transform 0.3s ease, background-size 0.3s ease; /* Add transition */
  padding: 10px; /* Add padding */
}

.card.big {
  width: 400px;
  height: 600px;
  transform: scale(1.05); /* Add scaling effect */
  background-size: 90%; /* Adjust background size */
}

.cardTitle {
  background-color: rgba(0, 0, 0, 0.5); /* Dark background */
  color: white; /* White text */
  padding: 10px; /* Add padding */
  border-radius: 5px; /* Rounded corners */
  text-align: center; /* Center text */
  font-weight: bold; /* Bold text */
  font-size: 1.2em; /* Increase font size */
}

.cardContent {
  background-color: rgba(0, 0, 0, 0.7); /* Darker background */
  color: white; /* White text */
  padding: 20px; /* Add padding */
  border-radius: 10px; /* More rounded corners */
  text-align: center; /* Center text */
  display: flex;
  flex-direction: column;
  align-items: center;
}

.cardContent h2 {
  font-size: 1.8em; /* Increase font size */
  margin-bottom: 10px; /* Add space below heading */
}

.cardContent p {
  font-size: 1.1em; /* Increase font size */
  line-height: 1.5; /* Improve line spacing */
}

.readMoreLink {
  margin-top: 10px;
  font-size: 1.2em;
  color: white;
  text-decoration: none;
  transition: color 0.3s ease;
}

.readMoreLink:hover {
  color: rgba(15, 95, 220, 1);
}

.arrow {
  font-size: 0.8em; /* Adjust arrow size */
  transition: transform 0.2s ease; /* Add transition */
}

.arrow:hover {
  transform: translateX(5px); /* Add sliding effect */
}
