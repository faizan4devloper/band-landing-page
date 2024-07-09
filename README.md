import React from "react";
import styles from "./AllCardsPage.module.css"; // Create a new CSS module for styling
import Cards from "./Cards";
import { cardsData } from "../../data"; // Import cardsData from the data file
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";

const AllCardsPage = ({ cardsData }) => {
  return (
    <div className={styles.allCardsPage}>
      <div className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} className={styles.backIcon} />
        <span>Back</span>
      </div>
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
    </div>
  );
};

export default AllCardsPage;




.allCardsPage {
  padding: 20px;
}

.backButton {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.backIcon {
  font-size: 20px;
  margin-right: 10px;
}

.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(5, 1fr); /* Five cards per row */
  gap: 20px;
  padding-top: 20px;
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
