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
      items: ["LSH", "BFSI", "ENU", "Automotive", "GOVT"]
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
      <div className={styles.contentContainer}>
        <div className={styles.header}>
          <button onClick={handleBackButtonClick} className={styles.backButton}>
            <FontAwesomeIcon icon={faArrowLeft} />
          </button>
          <h1 className={styles.catalogsHeading}>All Solutions Catalog</h1>
        </div>
        <div className={styles.cardsContainer} ref={cardsContainerRef}>
          {filteredCards.map((card, index) => (
            <div key={index} className={`${styles.card} ${index === bigIndex ? styles.bigCard : ''}`}>
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




.allCardsPage {
  display: flex;
  padding: 20px;
}

.contentContainer {
  flex: 1;
  margin-left: 30px;
}

.header {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.backButton {
  background-color: rgba(230, 235, 245, 1);
  padding: 7px;
  border-radius: 4px;
  width: 32px;
  font-size: 18px;
  border: none;
  cursor: pointer;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}

.catalogsHeading {
  font-size: 24px;
  margin-left: 10px;
}

.cardsContainer {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
}

.card {
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  overflow: hidden;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.card img {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 8px 8px 0 0;
}

.card-content {
  padding: 15px;
}

.card-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}

.card-description {
  font-size: 14px;
  color: #555;
}

.bigCard {
  grid-column-end: span 2;
  grid-row-end: span 2;
}
