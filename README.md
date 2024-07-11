.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 20px; /* Adjust the gap space between cards */
  padding-top: 50px;
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
