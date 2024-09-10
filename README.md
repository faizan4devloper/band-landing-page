import React, { useState, useRef } from "react";
import { Link } from "react-router-dom";
import { useTheme } from '../components/Context/ThemeProvider'; // Import your ThemeProvider context
import styles from "./AllCardsPage.module.css";
import Card from "./Cards";
import CategorySidebar from "./CategorySidebar";
import SearchBar from "./SearchBar";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import IndustrySVG from "./CardsImages/industry.svg";
import BusinessSVG from "./CardsImages/business.svg";
import NoResultsImage from "./CardsImages/thinking-face-svgrepo-com.svg";

const AllCardsPage = ({ cardsData }) => {
  const { theme } = useTheme(); // Get the current theme
  const [bigIndex, setBigIndex] = useState(null);
  const [filteredCards, setFilteredCards] = useState(cardsData);
  const [searchQuery, setSearchQuery] = useState("");
  const [activeItems, setActiveItems] = useState({}); // For keeping track of active filters
  const cardsContainerRef = useRef(null);

  const handleBackButtonClick = () => {
    window.history.back();
  };

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleFilterChange = (updatedActiveItems) => {
    setActiveItems(updatedActiveItems);

    const industryFilters = updatedActiveItems["Industry"] || [];
    const businessFunctionFilters = updatedActiveItems["Business Function"] || [];

    const filtered = cardsData.filter((card) => {
      const matchesIndustry =
      industryFilters.length === 0 ||
      industryFilters.includes("All") || // If "All" is selected, show all cards for Industry
      industryFilters.includes(card.industry);

    const matchesBusinessFunction =
      businessFunctionFilters.length === 0 ||
      businessFunctionFilters.includes("All") || // If "All" is selected, show all cards for Business Function
      businessFunctionFilters.includes(card.businessFunction);

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

  const generateTags = (card) => {
    const tags = [];
    if (activeItems["Industry"]?.includes(card.industry) && card.industry !== "All") {
      tags.push(card.industry);
    }
    if (activeItems["Business Function"]?.includes(card.businessFunction) && card.businessFunction !== "All") {
      tags.push(card.businessFunction);
    }
    return tags;
  };

  const getLastRowStartIndex = () => {
    const totalCards = filteredCards.length;
    const cardsPerRow = 4;
    const remainder = totalCards % cardsPerRow;
    return remainder === 0 ? totalCards - cardsPerRow : totalCards - remainder;
  };

  const lastRowStartIndex = getLastRowStartIndex();

  return (
    <div className={`${styles.allCardsPage} ${theme === 'dark' ? styles.darkTheme : ''}`}>
      <CategorySidebar categories={categories} onFilterChange={handleFilterChange} />
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h4 className={styles.catalogsHeading}>GenAI Solution Catalog</h4>
      <SearchBar query={searchQuery} onQueryChange={handleSearch} />
      <div className={styles.mainContainerCards}>
        <div className={styles.allCardsContainer} ref={cardsContainerRef}>
          {filteredCards.length > 0 ? (
            filteredCards.map((card, index) => (
              <div
                key={index}
                className={index >= lastRowStartIndex ? styles.lastRowCard : ""}
              >
                <Card
                  imageUrl={card.imageUrl}
                  title={card.title}
                  description={card.description}
                  isBig={index === bigIndex}
                  toggleSize={() => toggleSize(index)}
                  tags={generateTags(card)} // Pass tags here
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



/* AllCardsPage.module.css */
.allCardsPage {
  --background-color: #fff; /* Default light theme background */
  --text-color: #000; /* Default light theme text color */
}

.darkTheme .allCardsPage {
  --background-color: #333; /* Dark theme background */
  --text-color: #fff; /* Dark theme text color */
}

.allCardsPage {
  background-color: var(--background-color);
  color: var(--text-color);
  /* other styles */
}

.backButton {
  /* styles */
}

/* Define other styles for light and dark themes */
