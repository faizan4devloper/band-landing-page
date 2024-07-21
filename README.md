const cardsData = [
  {
    imageUrl: "image1.jpg",
    title: "Intelligent Assist",
    description: "Description for Intelligent Assist",
    industry: "LSH",
    businessFunction: "Customer Support"
  },
  {
    imageUrl: "image2.jpg",
    title: "Smart Analytics",
    description: "Description for Smart Analytics",
    industry: "BFSI",
    businessFunction: "Finance"
  },
  // Add more cards as needed
];


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

  const handleFilterChange = (activeItems) => {
    if (
      Object.values(activeItems).every((items) => items.length === 0) ||
      activeItems["Industry"]?.includes("All") ||
      activeItems["Business Function"]?.includes("All")
    ) {
      setFilteredCards(cardsData);
    } else {
      const filtered = cardsData.filter((card) =>
        Object.entries(activeItems).every(([category, items]) => {
          if (category === "Industry") {
            return items.includes(card.industry);
          } else if (category === "Business Function") {
            return items.includes(card.businessFunction);
          }
          return true;
        })
      );
      setFilteredCards(filtered);
    }
  };

  const categories = [
    {
      name: "Industry",
      svgIcon: IndustrySVG,
      items: ["All", "LSH", "BFSI", "ENU", "Automative", "GOVT"],
    },
    {
      name: "Business Function",
      svgIcon: BusinessSVG,
      items: ["All", "SDLC", "HR", "Customer Support", "Finance", "Customer Experience"],
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




import React, { useState } from "react";
import styles from "./CategorySidebar.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp, faTimes } from "@fortawesome/free-solid-svg-icons";

const CategorySidebar = ({ categories, onFilterChange }) => {
  const [openCategory, setOpenCategory] = useState(null);
  const [activeItems, setActiveItems] = useState({});

  const toggleCategory = (index) => {
    setOpenCategory(index === openCategory ? null : index);
  };

  const handleItemClick = (category, item) => {
    const isActive = activeItems[category]?.includes(item);
    const updatedActiveItems = {
      ...activeItems,
      [category]: isActive
        ? activeItems[category].filter((i) => i !== item)
        : [...(activeItems[category] || []), item],
    };
    setActiveItems(updatedActiveItems);
    onFilterChange(updatedActiveItems);
  };

  const handleRemoveFilter = (category, item) => {
    const updatedActiveItems = {
      ...activeItems,
      [category]: activeItems[category].filter((i) => i !== item),
    };
    setActiveItems(updatedActiveItems);
    onFilterChange(updatedActiveItems);
  };

  return (
    <div className={styles.sidebar}>
      <p className={styles.sideHead}>Explore by</p>
      <div className={styles.selectedFilters}>
        {Object.entries(activeItems).flatMap(([category, items]) =>
          items.map((item) => (
            <div key={`${category}-${item}`} className={styles.selectedFilter}>
              <span>{item}</span>
              <FontAwesomeIcon
                icon={faTimes}
                className={styles.removeIcon}
                onClick={() => handleRemoveFilter(category, item)}
              />
            </div>
          ))
        )}
      </div>
      {categories.map((category, index) => (
        <div key={index} className={styles.category}>
          <div
            className={`${styles.categoryHeader} ${openCategory === index ? styles.activeCategory : ""}`}
            onClick={() => toggleCategory(index)}
          >
            <img
              src={category.svgIcon}
              alt={`${category.name} icon`}
              className={`${styles.svgIcon} ${openCategory === index ? styles.activeIcon : ""}`}
            />
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
                  onClick={() => handleItemClick(category.name, item)}
                >
                  <input
                    type="checkbox"
                    checked={activeItems[category.name]?.includes(item)}
                    readOnly
                    className={styles.checkbox}
                  />
                  <span className={styles.itemText}>{item}</span>
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
