import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize, tags }) => {
  const [isHovered, setIsHovered] = useState(false);
  const [showPopup, setShowPopup] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  // Merge tags into a single string separated by "/"
  const mergedTags = tags && tags.length > 1 ? tags.join(" / ") : tags?.[0];

  // List of titles where the popup should be shown instead of navigating
  const noReadMoreTitles = [
    "AAIG-API Analyzer & Insight Generator",
    "Responsible Gen AI with Llama-13 B",
    "Graph data Interpretation using Gen AI",
    "Predictive Asset Maintenance​(PAM)",
    "API based Test Case Generation",
    "SOP Assistance",
    "AMS Support Automation",
  ];

  const handleReadMoreClick = (e) => {
    if (noReadMoreTitles.includes(title)) {
      e.preventDefault(); // Prevent navigation
      setShowPopup(true); // Show the pop-up
    }
  };

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {mergedTags && (
          <div className={styles.tagsContainer}>
            <span className={styles.tag}>{mergedTags}</span>
          </div>
        )}
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h3>{title}</h3>
            <p>{description}</p>
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}`,
              }}
              className={styles.readMoreLink}
              onClick={handleReadMoreClick}
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
        {showPopup && (
          <div className={styles.cardPopup} onClick={(e) => e.stopPropagation()}>
            <h2>No Data Available</h2>
            <p>Solution Coming Soon!</p>
            <button className={styles.closeButton} onClick={() => setShowPopup(false)}>
              Close
            </button>
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;



/* Styles for the pop-up within the card */
.cardPopup {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  text-align: center;
  width: 80%; /* Adjust width to fit within the card */
  z-index: 10; /* Ensure the pop-up appears above other content */
  animation: fadeIn 0.3s ease-in-out;
}

/* Close button inside the pop-up */
.closeButton {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
  margin-top: 10px;
}

.closeButton:hover {
  background-color: #0056b3;
}

/* Fade-in animation for the pop-up */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translate(-50%, -45%);
  }
  to {
    opacity: 1;
    transform: translate(-50%, -50%);
  }
}
