import React, { useState, useRef } from "react";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards";
import CategorySidebar from "./CategorySidebar";
import SearchBar from "./SearchBar"; // Import SearchBar
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft, faSearch } from "@fortawesome/free-solid-svg-icons"; // Import the search icon

import BusinessSVG from "./business.svg";
import IndustrySVG from "./industry.svg";
import NoResultsImage from "./thinking-face-svgrepo-com.svg";

const AllCardsPage = ({ cardsData }) => {
  const [bigIndex, setBigIndex] = useState(null);
  const [filteredCards, setFilteredCards] = useState(cardsData);
  const [searchQuery, setSearchQuery] = useState(""); // Add search query state
  const [activeFilters, setActiveFilters] = useState({}); // Track active filters
  const cardsContainerRef = useRef(null);

  const handleBackButtonClick = () => {
    window.history.back();
  };

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleFilterChange = (activeItems) => {
    setActiveFilters(activeItems);
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
                  industry={card.industry}
                  businessFunction={card.businessFunction}
                  showTags={Object.keys(activeFilters).length > 0} // Show tags only if filters are active
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



import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";
import styles from "./Cards.module.css";

const Card = ({ imageUrl, title, description, isBig, toggleSize, industry, businessFunction, showTags }) => {
  const [isHovered, setIsHovered] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  return (
    <div className={styles.cardContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h3>{title}</h3>
            <p>{description}</p>
            {showTags && (
              <div className={styles.tags}>
                {industry && <span className={styles.tagIndustry}>{industry}</span>}
                {businessFunction && <span className={styles.tagBusiness}>{businessFunction}</span>}
              </div>
            )}
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}`,
              }}
              className={styles.readMoreLink}
            >
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${isHovered ? styles.hovered : ""}`}
                onMouseEnter={() => setIsHovered(true)}
                onMouseLeave={() => setIsHovered(false)}
              >
                <span style={{ fontSize: isHovered ? "0.8em" : "1em" }}>
                  {isHovered && "Read More "}
                </span>
                <span style={{ marginLeft: isHovered ? "5px" : "0", fontSize: isHovered ? "1.3em" : "1em" }}>
                  <FontAwesomeIcon icon={faArrowRight} />
                </span>
              </span>
            </Link>
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;
