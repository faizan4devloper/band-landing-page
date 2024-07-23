import React, { useState, useRef } from "react";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards";
import CategorySidebar from "./CategorySidebar";
import SearchBar from "./SearchBar"; // Import SearchBar
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft, faSearch } from "@fortawesome/free-solid-svg-icons"; // Import the search icon

import BusinessSVG from "./business.svg";
import IndustrySVG from "./industry.svg";
import NoResultsImage from "./no-results.svg"; // Import an image for no results

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
              <p className={styles.noResults}>No results found</p>
            </div>
          )}
        </div>
      </div>
    </div>
  );
};

export default AllCardsPage;




.allCardsPage {
  padding: 20px;
  margin-top: 112px;
  margin-left: 270px; /* Make space for the sidebar */
  position: fixed;
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
  grid-template-columns: repeat(4, 1fr);
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px);
  overflow-y: auto;

  /* Beautiful scrollbar customization */
  scrollbar-width: thin;
  scrollbar-color: rgba(95, 30, 193, 0.8) transparent; /* Adjust scrollbar colors as needed */
}

.allCardsContainer > div {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px; /* Optional: Add some padding for spacing */
  box-sizing: border-box;
}

.lastRowCard {
  padding-bottom: 50px !important;
}

.catalogsHeading {
  font-size: 18px; /* Adjust font size as needed */
  margin: 0 10px; /* Adjust spacing from the back button */
  position: fixed;
  top: 82px; /* Adjust vertical position */
  left: 190px; /* Adjust horizontal position */
}

.noResultsContainer {
  grid-column: span 4;
  text-align: center;
  color: #888;
  font-size: 18px;
  padding: 20px;
}

.noResultsImage {
  width: 150px; /* Adjust size as needed */
  height: auto;
  margin-bottom: 20px; /* Space between image and text */
}

.noResults {
  font-size: 24px; /* Larger font size for the message */
  color: rgba(95, 30, 193, 1); /* Matching color theme */
  margin: 0;
}
