// AllCardsPage.js
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
          <div key={index} className={styles.cardWrapper}>
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
  );
};

export default AllCardsPage;





/* AllCardsPage.module.css */
.allCardsPage {
  padding: 20px;
  margin-top: 40px;
}

.backButton {
  background-color: rgba(230, 235, 245, 1);
  padding: 8px;
  margin-bottom: 10px;
  border-radius: 4px;
  width: 40px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  margin-right: 30px;
  position: fixed;
  left: 40px;
}

.backIcon {
  font-size: 12px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}

.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(5, 1fr); /* Five cards per row */
  gap: 20px; /* Adjust as needed for spacing between cards */
  padding-top: 50px;
}

@media (max-width: 1200px) {
  .allCardsContainer {
    grid-template-columns: repeat(4, 1fr); /* Four cards per row on smaller screens */
  }
}

@media (max-width: 900px) {
  .allCardsContainer {
    grid-template-columns: repeat(3, 1fr); /* Three cards per row on even smaller screens */
  }
}

@media (max-width: 600px) {
  .allCardsContainer {
    grid-template-columns: repeat(2, 1fr); /* Two cards per row on mobile devices */
  }
}

@media (max-width: 400px) {
  .allCardsContainer {
    grid-template-columns: 1fr; /* One card per row on very small screens */
  }
}

/* Styles specifically for cards within AllCardsPage */
.cardWrapper {
  border: 1px solid rgba(0, 0, 0, 0.1); /* Example border style */
  padding: 10px;
  margin: 10px;
  text-align: center;
}

.cardWrapper:hover {
  border-color: rgba(95, 30, 193, 1); /* Change border color on hover */
}
