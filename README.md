// data.js
import image1 from "./components/Cards/card1.jpg";
import image2 from "./components/Cards/card2.jpg";
import image3 from "./components/Cards/card3.jpg";
import image4 from "./components/Cards/card4.jpg";
import image5 from "./components/Cards/card5.jpg";

export const cardsData = [
  {
    imageUrl: image1,
    title: "Email EAR",
    description:
      "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
  },
  {
    imageUrl: image2,
    title: "Signature extraction & verification",
    description:
      "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
  },
  {
    imageUrl: image3,
    title: "Intelligent Assist (Q & A)",
    description:
      "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
  },
  {
    imageUrl: image4,
    title: "Case Intelligent",
    description:
      "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
  },
  {
    imageUrl: image5,
    title: "Code Great",
    description:
      "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
  },
];







import React from "react";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";

export const Home = ({ cardsData }) => {
  return (
    <div className={styles.cardsContainer}>
      {cardsData.map((card, index) => (
        <Cards
          key={index}
          imageUrl={card.imageUrl}
          title={card.title}
          description={card.description}
        />
      ))}
    </div>
  );
};





// components/Cards/Cards.js
import React, { useState } from "react";
import { Link } from "react-router-dom"; // Import Link component
import styles from "./Cards.module.css"; // Import CSS module for card styles

const Card = ({ imageUrl, title, description, isBig, toggleSize }) => {
  const [isHovered, setIsHovered] = useState(false);

  return (
    <div className={styles.cardsContainer}>
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
            {/* Add Link component to navigate to dashboard with title query parameter */}
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}`,
              }}
              className={styles.readMoreLink}
            >
              {/* Content of the "Read More" link */}
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${
                  isHovered ? styles.hovered : ""
                }`}
                onMouseEnter={() => setIsHovered(true)}
                onMouseLeave={() => setIsHovered(false)}
              >
                <span
                  style={{
                    fontSize: isHovered ? "0.8em" : "1em", // Decrease font size on hover
                  }}
                >
                  {isHovered && "Read More "}
                </span>
                <span
                  style={{
                    marginLeft: isHovered ? "5px" : "0", // Add space on hover
                    fontSize: isHovered ? "1.3em" : "1em", // Increase size on hover
                  }}
                >
                  &#129106;
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
