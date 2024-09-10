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
  width: 770px;
  padding: 0px 20px;
  background-color: #ffffff;
  /*background: linear-gradient(to bottom, #1a1a2e, #16213e);*/

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

.noResultsContainer {
  grid-column: span 4;
  text-align: center;
  padding: 40px;
  background-color: #f4f4f4; /* Lighter background for contrast */
  border: 2px dashed #ddd; /* Dashed border to highlight the section */
  border-radius: 8px; /* Rounded corners for a modern look */
  color: #666; /* Slightly darker text color for better readability */
  font-size: 16px; /* Adjust font size for balance */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
}

.noResultsImage {
  width: 100px; /* Slightly larger image */
  height: auto;
  margin-bottom: 20px; /* Space between image and text */
}

.noResults {
  font-size: 24px; /* Larger font size for emphasis */
  color: #333; /* Darker text color for contrast */
  margin: 0;
}

.noResults p {
  font-size: 18px; /* Slightly larger font size for additional message */
  color: #555; /* A softer color for additional message text */
  margin: 10px 0 0;
}

.noResults a {
  color: #007bff; /* Blue color for links */
  text-decoration: none; /* Remove underline */
  font-weight: bold; /* Bold text for visibility */
}

.noResults a:hover {
  text-decoration: underline; /* Underline on hover for better UX */
}


