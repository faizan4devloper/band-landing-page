import React, { useState, useRef } from "react";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards";
import CategorySidebar from "./CategorySidebar";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";

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
    { name: "Industry", items: ["LSH", "BFSI", "ENU","Automative","GOVT"] },
    { name: "Buisness Functions", items: ["SDLC", "HR", "Customer Support", "Finance", "Customer Experience", "Responsible AI"] },
    { name: "Category 3", items: ["Type E", "Type F"] },
    { name: "Category 4", items: ["Type G", "Type H"] },
  ];

  return (
    <div className={styles.allCardsPage}>
      <CategorySidebar categories={categories} onFilterChange={handleFilterChange} />
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
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
  );
};

export default AllCardsPage;



.allCardsPage {
  /*padding: 20px;*/
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
  top: 75px;
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
  position: fixed;
  gap: 0; /* No gap, as borders will create the separation */
  /*padding-top: 50px;*/
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
  margin-top: 50px;
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 50px;
  padding-right: 0px;
  margin-left: 20px;
}

.allCardsContainer > div {
  border-radius: 4px;
  border: 1px solid #D3D3D3; /* Border around each card */
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px; /* Optional: Add some padding for spacing */
  box-sizing: border-box;
  
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



.allCardsPage {
  padding: 20px;
  margin-top: 40px;
  margin-left: 270px; /* Make space for the sidebar */
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
  left: 30px;
  top: 68px;
}

.backIcon {
  font-size: 12px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}

.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(4, 1fr); /* Five cards per row */
  gap: 0; /* No gap, as borders will create the separation */
  /*margin-top: 10px;*/
}

.allCardsContainer > div {
  border-radius: 4px;
  border: 1px solid #D3D3D3; /* Border around each card */
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px; /* Optional: Add some padding for spacing */
  box-sizing: border-box;
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
