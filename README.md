import React, { useState } from "react";
import styles from "./AllCardsPage.module.css";
import Cards from "./Cards";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";

const AllCardsPage = ({ cardsData, cardsContainerRef }) => {
  const [bigIndex, setBigIndex] = useState(null);

  const handleBackButtonClick = () => {
    window.history.back();
  };

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  return (
    <div className={styles.allCardsPageContainer}>
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <div className={styles.sidebar}>
        <h2>Explore By: Industry</h2>
        <div className={styles.categories}>
          <label>
            <input type="checkbox" name="industry1" />
            Industry 1
          </label>
          <label>
            <input type="checkbox" name="industry2" />
            Industry 2
          </label>
          <label>
            <input type="checkbox" name="industry3" />
            Industry 3
          </label>
          <label>
            <input type="checkbox" name="industry4" />
            Industry 4
          </label>
        </div>
      </div>
      <div className={styles.allCardsPage}>
        <div className={styles.allCardsContainer} ref={cardsContainerRef}>
          {cardsData.map((card, index) => (
            <div key={index}>
              <Cards
                imageUrl={card.imageUrl}
                title={card.title}
                description={card.description}
                isBig={index === bigIndex}
                toggleSize={() => toggleSize(index)}
              />
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};

export default AllCardsPage;


.allCardsPageContainer {
  display: flex;
  padding: 20px;
}

.allCardsPage {
  flex-grow: 1;
  padding: 20px;
  margin-top: 40px;
  margin-left: 250px; /* Adjusted for sidebar space */
}

.backButton {
  background-color: rgba(230, 235, 245, 1);
  padding: 8px;
  margin-bottom: 10px;
  border-radius: 4px;
  width: 40px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  margin-right: 30px;
  position: fixed;
  left: 40px;
  top: 75px;
}

.backIcon {
  font-size: 12px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1);
}

.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 0;
  padding-top: 50px;
}

.allCardsContainer > div {
  border-radius: 4px;
  border: 1px solid #D3D3D3;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
  box-sizing: border-box;
}

@media (max-width: 1200px) {
  .allCardsContainer {
    grid-template-columns: repeat(4, 1fr);
  }
}

@media (max-width: 900px) {
  .allCardsContainer {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 600px) {
  .allCardsContainer {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 400px) {
  .allCardsContainer {
    grid-template-columns: 1fr;
  }
}

.sidebar {
  width: 200px;
  position: fixed;
  top: 100px;
  left: 20px;
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.sidebar h2 {
  font-size: 18px;
  margin-bottom: 10px;
}

.categories label {
  display: block;
  margin-bottom: 8px;
}

.categories input[type="checkbox"] {
  margin-right: 8px;
}
