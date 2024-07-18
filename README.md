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
    items: ["Manufacturing", "Healthcare", "Retail"]
  },
  {
    name: "Business Function",
    svgIcon: BusinessSVG, // Path to the SVG icon
    items: ["Finance", "Human Resources", "Marketing"]
  },
  // Add other categories as needed
];

  return (
    <div className={styles.allCardsPage}>
      <CategorySidebar categories={categories} onFilterChange={handleFilterChange} />
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <div className={styles.mainContainerCards}>
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
    </div>
  );
};

export default AllCardsPage;

.allCardsPage {
  padding: 20px;
  margin-top: 90px;
  margin-left: 270px; /* Make space for the sidebar */
  
  
  
}

.backButton {
  background-color: rgba(230, 235, 245, 1);
  padding: 7px;
  margin-bottom: 10px;
  border-radius: 4px;
  width: 32px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  margin-right: 30px;
  margin-top: 10px;
  position: fixed;
  left: 156px;
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
  grid-template-columns: repeat(4, 1fr); /* Adjust number of cards per row */
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 50px;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
}

.allCardsContainer > div {
  /*border-radius: 4px;*/
  /*border: 0.1px solid #d3d3d3; */
  /*box-shadow: 0px 2px 5px #00000029 !important;*/
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px; /* Optional: Add some padding for spacing */
  box-sizing: border-box;
}


/*.mainContainerCards{*/
/*  position: fixed*/
/*}*/

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
