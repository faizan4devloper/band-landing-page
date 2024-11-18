import React, { forwardRef } from "react";
import { Link } from "react-router-dom";
import styles from "./FeaturesCard.module.css";

const FeaturesCard = forwardRef(({ icon, title, description, buttonLabel, to }, ref) => {
  return (
    <div ref={ref} className={styles.card}>
      <div className={styles.icon}>{icon}</div>
      <h3>{title}</h3>
      <p>{description}</p>
      <Link to={to} className={styles.button}>
        {buttonLabel} <span className={styles.arrow}>&rarr;</span>
      </Link>
    </div>
  );
});

export default FeaturesCard;


/* Card Styling */
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

/* Button Styling */
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 1rem;
  color: #ffffff;
  background-color: #4a90e2;
  border-radius: 50px;
  text-decoration: none;
  transition: background-color 0.3s ease;
  cursor: pointer;
}

.button:hover {
  background-color: #357abd;
}

/* Arrow Animation */
.arrow {
  margin-left: 10px;
  transition: transform 0.3s ease;
}

.button:hover .arrow {
  transform: translateX(5px);
}





import React, { useEffect, useRef } from "react";
import FeaturesCard from "./FeaturesCard";
import styles from "./FeaturesSection.module.css";
import { gsap } from "gsap";

const FeaturesSection = () => {
  const cardsRef = useRef([]);

  useEffect(() => {
    // Initial animation for the cards
    gsap.from(cardsRef.current, {
      opacity: 0,
      y: 50,
      duration: 1,
      stagger: 0.2,
      ease: "power2.out",
    });
  }, []);

  return (
    <section className={styles.featuresSection}>
      <h2>Our Features</h2>
      <div className={styles.cards}>
        <FeaturesCard
          ref={(el) => (cardsRef.current[0] = el)}
          icon="📄"
          title="Manage Product Sheets"
          description="Easily manage and organize product sheets for your business."
          buttonLabel="Go to Product Sheets"
          to="/product-sheets"
        />
        <FeaturesCard
          ref={(el) => (cardsRef.current[1] = el)}
          icon="📂"
          title="Manage Claims"
          description="Streamline the claims process with efficient management tools."
          buttonLabel="Go to Claims"
          to="/claims"
        />
      </div>
    </section>
  );
};

export default FeaturesSection;


.featuresSection {
  padding: 40px 80px;
  background-color: #ffffff;
}

.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

h2 {
  font-size: 2rem;
  color: #333;
  text-align: center;
  margin-bottom: 20px;
}



import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Header from "./components/Header/Header";
import WelcomePage from "./components/WelcomePage";
import ProductSheetsPage from "./components/ProductSheetsPage";
import ClaimsPage from "./components/ClaimsPage";

const App = () => {
  return (
    <Router>
      <Header />
      <Routes>
        <Route path="/" element={<WelcomePage />} />
        <Route path="/product-sheets" element={<ProductSheetsPage />} />
        <Route path="/claims" element={<ClaimsPage />} />
      </Routes>
    </Router>
  );
};

export default App;



import React from "react";

const ProductSheetsPage = () => {
  return <div>Manage Product Sheets Page</div>;
};

export default ProductSheetsPage;



import React from "react";

const ClaimsPage = () => {
  return <div>Manage Claims Page</div>;
};

export default ClaimsPage;
