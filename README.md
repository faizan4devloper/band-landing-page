import React, { useState } from "react";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";
import { Link } from "react-router-dom";

const Cards = ({ imageUrl, title, description, isBig, toggleSize }) => {
  const [isHovered, setIsHovered] = useState(false);

  return (
    <div className={styles.card}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h2>{title}</h2>
            <p>{description}</p>
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}`,
              }}
              className={styles.readMoreLink}
            >
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${
                  isHovered ? styles.hovered : ""
                }`}
                onMouseEnter={() => setIsHovered(true)}
                onMouseLeave={() => setIsHovered(false)}
              >
                <span
                  style={{
                    fontSize: isHovered ? "0.8em" : "1em",
                  }}
                >
                  {isHovered && "Read More "}
                </span>
                <span
                  style={{
                    marginLeft: isHovered ? "5px" : "0",
                    fontSize: isHovered ? "1.3em" : "1em",
                  }}
                >
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

export default Cards;


import React from "react";
import { Link } from "react-router-dom";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards";
import { cardsData } from "../../data"; // Import cardsData from data.js
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";

const AllCardsPage = () => {
  const handleBackButtonClick = () => {
    window.history.back(); // Go back to the previous page
  };

  return (
    <div className={styles.allCardsPage}>
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <div className={styles.cardsContainer}>
        {cardsData.map((card, index) => (
          <Cards
            key={index}
            imageUrl={card.imageUrl}
            title={card.title}
            description={card.description}
            isBig={false} // Ensure all cards are in the default small size
            toggleSize={() => {}} // If toggleSize is needed, you can pass a function here
          />
        ))}
      </div>
    </div>
  );
};

export default AllCardsPage;
