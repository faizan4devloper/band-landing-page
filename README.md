.allCardsPage {
  padding: 20px;
  margin-top: 112px;
  margin-left: 270px; /* Make space for the sidebar */
}

.backButton {
  background-color: rgba(230, 235, 245, 1);
  padding: 7px;
  margin-bottom: 10px;
  border-radius: 4px;
  width: 32px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  position: fixed;
  left: 156px;
  top: 68px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}

.catalogsHeading {
  font-size: 18px;
  margin: 0 10px;
  position: fixed;
  top: 82px;
  left: 190px;
}

.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px; /* Adjust gap between grid items */
  padding: 10px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 232px); /* Adjust height based on other fixed elements */
  overflow-y: auto;

  /* Beautiful scrollbar customization */
  scrollbar-width: thin;
  scrollbar-color: rgba(95, 30, 193, 0.8) transparent;
}

.allCardsContainer > div {
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
}

.cardContainer {
  transition: all 0.3s ease; /* Smooth transition for card size changes */
}

.bigCard {
  grid-column: span 2; /* Expand big card to occupy two grid columns */
  grid-row: span 2; /* Expand big card to occupy two grid rows */
}
