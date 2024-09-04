import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize, tags }) => {
  const [isHovered, setIsHovered] = useState(false);
  const [showTooltip, setShowTooltip] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  // Merge tags into a single string separated by "/"
  const mergedTags = tags && tags.length > 1 ? tags.join(" / ") : tags?.[0];

  // List of titles where the tooltip should be shown instead of navigating
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
      setShowTooltip(true); // Show the tooltip
      setTimeout(() => setShowTooltip(false), 3000); // Hide after 3 seconds
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
            {showTooltip && (
              <div className={styles.tooltip}>
                No Data Available<br />Solution Coming Soon!
              </div>
            )}
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;


/* Styles for the tooltip */
.tooltip {
  position: absolute;
  bottom: 110%; /* Adjust this value to move the tooltip above the "Read More" link */
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(0, 0, 0, 0.75);
  color: white;
  padding: 8px 12px;
  border-radius: 4px;
  white-space: nowrap;
  font-size: 0.9em;
  z-index: 10;
  opacity: 0;
  animation: fadeInTooltip 0.3s forwards;
}

@keyframes fadeInTooltip {
  from {
    opacity: 0;
    transform: translateX(-50%) translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }
}
