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
}

.allCardsContainer > div {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
  box-sizing: border-box;
  /* Optional: Add other styles as needed */
  /* Add scrollbar styling if necessary */
}

.cardScrollContainer {
  overflow-y: auto; /* Enable scrolling for the cards */
  height: 100%; /* Occupy full height of the container */
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
