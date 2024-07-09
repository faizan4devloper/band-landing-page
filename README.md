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
  /*gap: 10px;*/
  padding: 20px;
  padding-top: 120px;
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
