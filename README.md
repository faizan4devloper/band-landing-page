
import React, { forwardRef } from "react";
import styles from "./FeaturesCard.module.css";

const FeaturesCard = forwardRef(({ icon, title, description }, ref) => {
  return (
    <div ref={ref} className={styles.card}>
      <div className={styles.icon}>{icon}</div>
      <h3>{title}</h3>
      <p>{description}</p>
    </div>
  );
});

export default FeaturesCard;


.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
}

.icon {
  font-size: 2.5rem;
  color: #4a90e2;
  margin-bottom: 20px;
}

h3 {
  font-size: 1.2rem;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

p {
  font-size: 1rem;
  color: #666;
  line-height: 1.5;
}


import React from "react";
import HeroSection from "./HeroSection";
import FeaturesSection from "./FeaturesSection";
import styles from "./WelcomePage.module.css";

const WelcomePage = () => {
  return (
    <div className={styles.container}>
      <HeroSection />
      <FeaturesSection />
    </div>
  );
};

export default WelcomePage;


.container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: #f9f9f9;
}


import React from 'react';
import WelcomePage from './components/WelcomePage';
import Header from './components/Header/Header';

const App = ()=>{
  return <div>
    <Header/>
    <WelcomePage/>
  </div>
}

export default App;


.App {
  text-align: center;
}

