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
    <div className={styles.allCardsPage}>
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
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
  );
};

export default AllCardsPage;

.allCardsPage {
  padding: 20px;
  margin-top: 40px;
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
  top:75px;
}

.backIcon {
  font-size: 12px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}

.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(5, 1fr); /* Four cards per row */
  gap: 0; /* No gap, as borders will create the separation */
  padding-top: 50px;
  /*margin-top: 10px;*/
}

.allCardsContainer > div {
  border-radius: 4px;
  border: 1px solid #D3D3D3; /* Border around each card */
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px; /* Optional: Add some padding for spacing */
  box-sizing: border-box;
}

@media (max-width: 1200px) {
  .allCardsContainer {
    grid-template-columns: repeat(4, 1fr); /* Four cards per row on smaller screens */
  }
}

@media (max-width: 900px) {
  .allCardsContainer {
    grid-template-columns: repeat(3, 1fr); /* Three cards per row on even smaller screens */
  }
}

@media (max-width: 600px) {
  .allCardsContainer {
    grid-template-columns: repeat(2, 1fr); /* Two cards per row on mobile devices */
  }
}

@media (max-width: 400px) {
  .allCardsContainer {
    grid-template-columns: 1fr; /* One card per row on very small screens */
  }
}




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
    <div className={styles.allCardsPage}>
      <button onClick={handleBackButtonClick} className={styles.backButton}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <div className={styles.container}>
        <div className={styles.sidebar}>
          <h3>Explore By:</h3>
          <h4>Industry</h4>
          <div className={styles.checkboxGroup}>
            <label>
              <input type="checkbox" /> Industry 1
            </label>
            <label>
              <input type="checkbox" /> Industry 2
            </label>
            <label>
              <input type="checkbox" /> Industry 3
            </label>
            <label>
              <input type="checkbox" /> Industry 4
            </label>
          </div>
        </div>
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


.allCardsPage {
  padding: 20px;
  margin-top: 40px;
}

.container {
  display: flex;
}

.sidebar {
  width: 250px;
  padding: 20px;
  background-color: #f9f9f9;
  border-right: 1px solid #ddd;
  margin-right: 20px;
}

.sidebar h3,
.sidebar h4 {
  margin-bottom: 15px;
}

.checkboxGroup {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.checkboxGroup label {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 16px;
  cursor: pointer;
}

.checkboxGroup input {
  width: 20px;
  height: 20px;
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
  border: 1px solid #d3d3d3;
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
