import React, { forwardRef } from "react";
import { Link } from "react-router-dom";
import styles from "./FeaturesCard.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const FeaturesCard = forwardRef(({ icon, title, description, buttonLabel, to }, ref) => {
  return (
    <div ref={ref} className={styles.card}>
      <div className={styles.icon}>{icon}</div>
      <h3>{title}</h3>
      <p>{description}</p>
      <Link to={to} className={styles.button}>
        {buttonLabel} <span className={styles.arrow}><FontAwesomeIcon icon={faArrowRight}/></span>
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
  background:linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
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
