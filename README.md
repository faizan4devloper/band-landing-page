.allCardsPage {
  padding: 20px;
  margin-top: 120px;
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
  margin-right: 30px;
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
  scrollbar-radius:10px;

}


.allCardsContainer > div {
  /*border-radius: 4px;*/
  /*border: 0.1px solid #d3d3d3; */
  /*box-shadow: 0px 2px 5px #00000029 !important;*/
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px; /* Optional: Add some padding for spacing */
  box-sizing: border-box;
}


/*.mainContainerCards{*/
/*  position: fixed*/
/*}*/

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
