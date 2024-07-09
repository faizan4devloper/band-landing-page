import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize }) => {
  const [isHovered, setIsHovered] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

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

export default Card;



.cardsContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 20px;
}

.card {
  width: 160px;
  overflow: hidden;
  height: 160px;
  border-radius: 12px;
  background-size: cover;
  background-position: center;
  cursor: pointer;
  position: relative;
  margin-bottom: 60px;
  transition: transform 0.6s ease;
}
.card:hover {
  transform: translate(0, -8px);
}

.big {
  width: 190px;
  height: 190px;
  z-index: 0;
  transform: scale(1.1);
}

.cardTitle {
  position: absolute;
  font-family: "Poppins", sans-serif;
  font-size: 12px;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 75px;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000000 100%),
    linear-gradient(0deg, rgba(23, 23, 25, 0.3), rgba(23, 23, 25, 0.3));
  backdrop-filter: blur(10px);
  border-radius: 12px;
  color: white;
  padding: 18px;
  box-sizing: border-box;
  transition: opacity 0.3s ease;
  text-align: center;
}

.cardContent {
  position: absolute;
  font-family: "Poppins", sans-serif;
  font-size: 8px;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 140px;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000000 100%),
    linear-gradient(0deg, rgba(23, 23, 25, 0.3), rgba(23, 23, 25, 0.3));
  border-radius: 12px;
  backdrop-filter: blur(10px);
  color: white;
  padding: 18px;
  box-sizing: border-box;
  transform: translateY(100%);
  transition: transform 0.3s ease, background 0.3s ease;
}

.cardContent.slide-up {
  transform: translateY(0);
}

.cardContent::before {
  content: "";
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to top, #6f36cd, #1f77f6);
  transition: top 0.3s ease;
  border-radius: 12px;
  z-index: -1;
}
.arrow {
  cursor: pointer;
  position: absolute;
  display: flex;
  bottom: 10px;
  right: 10px;
  font-size: 14px;
  width: 7px;
  height: 23px;
  padding: 2px 12px 0px 6px;
  border-radius: 50px;
  border: 2px solid rgba(255, 255, 255, 1);
  color: rgba(255, 255, 255, 1);
  overflow: hidden;
  transition: width 0.3s ease, background 0.5s ease, color 0.5s ease;
}

.arrow:hover {
  width: 80px;
  align-items: center;
  font-size: 10px;
  background-color: rgba(255, 255, 255, 1);
  color: rgba(15, 95, 220, 1);
}

.arrow.hovered {
  width: 65px;
}

.card:hover .cardContent::before {
  top: 0;
}

.readMore {
  left: 10px;
}

.card:hover .cardContent::before {
  background: linear-gradient(0deg, #6f36cd 0%, #1f77f6 100%);
}

.big .cardContent {
  transform: translateY(0);
}
