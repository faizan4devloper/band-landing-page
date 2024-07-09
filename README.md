// src/components/Cards/Cards.js
import React, { useState } from "react";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";
import { Link } from "react-router-dom";

const Cards = ({ imageUrl, title, description, isBig, toggleSize, showReadMoreLink }) => {
  const [isHovered, setIsHovered] = useState(false);

  const handleMouseEnter = () => {
    setIsHovered(true);
  };

  const handleMouseLeave = () => {
    setIsHovered(false);
  };

  return (
    <div className={`${styles.card} ${isBig ? styles.big : ""}`} style={{ backgroundImage: `url(${imageUrl})` }}>
      {!isBig && <div className={styles.cardTitle}>{title}</div>}
      {isBig && (
        <div className={styles.cardContent}>
          <h2>{title}</h2>
          <p>{description}</p>
          {showReadMoreLink && (
            <Link to="/dashboard" className={styles.readMoreLink}>
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${isHovered ? styles.hovered : ""}`}
                onMouseEnter={handleMouseEnter}
                onMouseLeave={handleMouseLeave}
              >
                <span style={{ fontSize: isHovered ? "0.8em" : "1em" }}>{isHovered && "Read More "}</span>
                <FontAwesomeIcon icon={faArrowRight} style={{ marginLeft: isHovered ? "5px" : "0" }} />
              </span>
            </Link>
          )}
        </div>
      )}
    </div>
  );
};

export default Cards;


// src/components/Cards/AllCardsPage.js
import React from "react";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";

const AllCardsPage = ({ cardsData, cardsContainerRef }) => {
  const handleBackButtonClick = () => {
    window.history.back();
  };

  return (
    <div className={styles.allCardsPage}>
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <div className={styles.allCardsContainer} ref={cardsContainerRef}>
        {cardsData.map((card, index) => (
          <Cards
            key={index}
            imageUrl={card.imageUrl}
            title={card.title}
            description={card.description}
            isBig={false}
            showReadMoreLink={false} // Pass showReadMoreLink prop as false
          />
        ))}
      </div>
    </div>
  );
};

export default AllCardsPage;
