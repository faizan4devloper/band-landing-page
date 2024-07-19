import React, { useState } from "react";
import styles from "./CategorySidebar.module.css";

const CategorySidebar = ({ categories, onFilterChange }) => {
  const [selectedFilters, setSelectedFilters] = useState([]);

  const handleCheckboxChange = (category, item) => {
    const filter = `${category}-${item}`;
    let updatedFilters = [...selectedFilters];

    if (selectedFilters.includes(filter)) {
      updatedFilters = updatedFilters.filter(f => f !== filter);
    } else {
      updatedFilters.push(filter);
    }

    setSelectedFilters(updatedFilters);
    onFilterChange(updatedFilters);
  };

  return (
    <div className={styles.categorySidebar}>
      {categories.map((category, index) => (
        <div key={index} className={styles.categorySection}>
          <h4 className={styles.categoryTitle}>
            <img src={category.svgIcon} alt={category.name} className={styles.categoryIcon} />
            {category.name}
          </h4>
          {category.items.map((item, itemIndex) => (
            <div key={itemIndex} className={styles.checkboxItem}>
              <input
                type="checkbox"
                id={`${category.name}-${item}`}
                name={item}
                value={item}
                onChange={() => handleCheckboxChange(category.name, item)}
                checked={selectedFilters.includes(`${category.name}-${item}`)}
              />
              <label htmlFor={`${category.name}-${item}`}>{item}</label>
            </div>
          ))}
        </div>
      ))}
    </div>
  );
};

export default CategorySidebar;

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

  const handleFilterChange = (selectedFilters) => {
    if (selectedFilters.length === 0) {
      setFilteredCards(cardsData);
      return;
    }

    const filtered = cardsData.filter(card => 
      selectedFilters.some(filter => {
        const [category, type] = filter.split("-");
        return card.category === category && card.type === type;
      })
    );

    setFilteredCards(filtered);
  };

  const categories = [
    {
      name: "Industry",
      svgIcon: IndustrySVG,
      items: ["LSH", "BFSI", "ENU", "Automative", "GOVT"]
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
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h4 className={styles.catalogsHeading}>Gen AI Solution Catalog</h4>
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
