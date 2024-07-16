import React, { useState } from "react";
import styles from "./CategorySidebar.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";

const CategorySidebar = ({ categories, onFilterChange }) => {
  const [openCategory, setOpenCategory] = useState(null);

  const toggleCategory = (index) => {
    setOpenCategory(index === openCategory ? null : index);
  };

  return (
    <div className={styles.sidebar}>
      {categories.map((category, index) => (
        <div key={index} className={styles.category}>
          <div 
            className={styles.categoryHeader} 
            onClick={() => toggleCategory(index)}
          >
            {category.name}
            <FontAwesomeIcon 
              icon={openCategory === index ? faChevronUp : faChevronDown} 
              className={styles.chevronIcon} 
            />
          </div>
          {openCategory === index && (
            <div className={styles.dropdown}>
              {category.items.map((item, itemIndex) => (
                <div 
                  key={itemIndex} 
                  className={styles.dropdownItem} 
                  onClick={() => onFilterChange(category.name, item)}
                >
                  {item}
                </div>
              ))}
            </div>
          )}
        </div>
      ))}
    </div>
  );
};

export default CategorySidebar;


.sidebar {
  position: fixed;
  top: 110px;
  left: 115px;
  width: 200px;
  height: calc(100% - 75px);
  border-right: 1px solid rgba(219, 197, 255, 1);
  padding: 20px;
  overflow-y: auto;
}

.category {
  margin-bottom: 20px;
}

.categoryHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  font-size: 12px;
  padding: 10px 15px;
  background-color: rgba(230, 230, 230, 1);
  border-radius: 4px;
}

.chevronIcon {
  margin-left: 10px;
}

.dropdown {
  padding: 10px;
  background-color: rgba(250, 250, 250, 1);
  border-radius: 4px;
  margin-top: 10px;
}

.dropdownItem {
  font-size: 11px;
  padding: 5px;
  cursor: pointer;
}

.dropdownItem:hover {
  background-color: rgba(220, 220, 220, 1);
}





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
    { name: "Business Functions", items: ["SDLC", "HR", "Customer Support", "Finance", "Customer Experience", "Responsible AI"] },
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
  grid-template-columns: repeat(4, 1fr); /* Adjust number of cards per row */
  gap: 10px; /* Add gap for spacing between cards */
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
