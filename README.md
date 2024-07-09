import React, { useState } from "react";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards"; // Use the existing Cards component
import { differentCardsData } from "../../data/differentCardsData"; // Import different card data

const AllCardsPage = () => {
  const [bigIndex, setBigIndex] = useState(null);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  return (
    <div className={styles.allCardsContainer}>
      {differentCardsData.map((card, index) => (
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
  );
};

export default AllCardsPage;


/* AllCardsPage.module.css */
.allCardsContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  padding: 20px;
}
