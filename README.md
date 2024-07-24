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

import React, { useState, useRef } from "react";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards";
import CategorySidebar from "./CategorySidebar";
import SearchBar from "./SearchBar"; // Import SearchBar
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft, faSearch } from "@fortawesome/free-solid-svg-icons"; // Import the search icon

import BusinessSVG from "./business.svg";
import IndustrySVG from "./industry.svg";
// import NoResultsImage from "./no-results.svg"; // Import an image for no results
import NoResultsImage from "./thinking-face-svgrepo-com.svg"

const AllCardsPage = ({ cardsData }) => {
  const [bigIndex, setBigIndex] = useState(null);
  const [filteredCards, setFilteredCards] = useState(cardsData);
  const [searchQuery, setSearchQuery] = useState(""); // Add search query state
  const cardsContainerRef = useRef(null);

  const handleBackButtonClick = () => {
    window.history.back();
  };

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleFilterChange = (activeItems) => {
    const industryFilters = activeItems["Industry"] || [];
    const businessFunctionFilters = activeItems["Business Function"] || [];

    const filtered = cardsData.filter((card) => {
      const matchesIndustry = industryFilters.length === 0 || industryFilters.includes(card.industry) || industryFilters.includes("All");
      const matchesBusinessFunction = businessFunctionFilters.length === 0 || businessFunctionFilters.includes(card.businessFunction) || businessFunctionFilters.includes("All");
      return matchesIndustry && matchesBusinessFunction;
    });

    setFilteredCards(filtered);
  };

  const handleSearch = (query) => {
    setSearchQuery(query);
    const filtered = cardsData.filter(card =>
      card.title.toLowerCase().includes(query.toLowerCase()) ||
      card.description.toLowerCase().includes(query.toLowerCase())
    );
    setFilteredCards(filtered);
  };

  const categories = [
    {
      name: "Industry",
      svgIcon: IndustrySVG,
      items: ["All", "LSH", "BFSI", "ENU", "Automotive", "GOVT"],
    },
    {
      name: "Business Function",
      svgIcon: BusinessSVG,
      items: ["All", "SDLC", "HR", "Customer Support", "Finance", "Customer Experience"],
    },
  ];

  const getLastRowStartIndex = () => {
    const totalCards = filteredCards.length;
    const cardsPerRow = 4;
    const remainder = totalCards % cardsPerRow;
    return remainder === 0 ? totalCards - cardsPerRow : totalCards - remainder;
  };

  const lastRowStartIndex = getLastRowStartIndex();

  return (
    <div className={styles.allCardsPage}>
      <CategorySidebar categories={categories} onFilterChange={handleFilterChange} />
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h4 className={styles.catalogsHeading}>Gen AI Solution Catalog</h4>
      <SearchBar query={searchQuery} onQueryChange={handleSearch} /> {/* Add SearchBar */}
      <div className={styles.mainContainerCards}>
        <div className={styles.allCardsContainer} ref={cardsContainerRef}>
          {filteredCards.length > 0 ? (
            filteredCards.map((card, index) => (
              <div
                key={index}
                className={index >= lastRowStartIndex ? styles.lastRowCard : ""}
              >
                <Cards
                  imageUrl={card.imageUrl}
                  title={card.title}
                  description={card.description}
                  isBig={index === bigIndex}
                  toggleSize={() => toggleSize(index)}
                />
              </div>
            ))
          ) : (
            <div className={styles.noResultsContainer}>
  <img src={NoResultsImage} alt="No results" className={styles.noResultsImage} />
  <p className={styles.noResults}>No solutions found.</p>
</div>
          )}
        </div>
      </div>
    </div>
  );
};

export default AllCardsPage;
