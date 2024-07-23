import React from "react";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";

export const Home = ({ cardsData }) => {
  return (
    <div className={styles.cardsContainer}>
      {cardsData.map((card, index) => (
        <Cards
          key={index}
          imageUrl={card.imageUrl}
          title={card.title}
          description={card.description}
        />
      ))}
    </div>
  );
};
