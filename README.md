.allCardsPage {
  padding: 20px;
  margin-top: 112px;
  margin-left: 270px; /* Make space for the sidebar */
  position: fixed;
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
  margin-top: 10px;
  position: fixed;
  left: 156px;
  top: 68px;
}

.backIcon {
  font-size: 12px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}

.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px);
  overflow-y: auto;

  /* Beautiful scrollbar customization */
  scrollbar-width: thin;
  scrollbar-color: rgba(95, 30, 193, 0.8) transparent; /* Adjust scrollbar colors as needed */
}

.allCardsContainer > div {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px; /* Optional: Add some padding for spacing */
  box-sizing: border-box;
}

.lastRowCard {
  padding-bottom: 50px !important;
}

.catalogsHeading {
  font-size: 18px; /* Adjust font size as needed */
  margin: 0 10px; /* Adjust spacing from the back button */
  position: fixed;
  top: 82px; /* Adjust vertical position */
  left: 190px; /* Adjust horizontal position */
}

.noResults {
  grid-column: span 4;
  text-align: center;
  color: #5F1EC1;
  font-size: 24px;
  font-weight: bold;
  padding: 40px;
  background-color: #f0f0f0;
  border-radius: 8px;
  margin-top: 20px;
  animation: fadeIn 0.5s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
