import React, { forwardRef } from "react";
import { Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";
import styles from "./FeaturesCard.module.css";

const FeaturesCard = forwardRef(({ icon, title, description, buttonLabel, to }, ref) => {
  return (
    <div ref={ref} className={styles.card}>
      <div className={styles.iconWrapper}>
        <div className={styles.icon}>{icon}</div>
      </div>
      <h3 className={styles.title}>{title}</h3>
      <p className={styles.description}>{description}</p>
      <Link to={to} className={styles.button}>
        {buttonLabel} <span className={styles.arrow}><FontAwesomeIcon icon={faArrowRight} /></span>
      </Link>
    </div>
  );
});

export default FeaturesCard;





/* Card Container */
.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 24px;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.card:hover {
  transform: translateY(-8px) scale(1.02);
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.25);
}

/* Icon Wrapper */
.iconWrapper {
  width: 70px;
  height: 70px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, rgba(95, 30, 193, 0.8), rgba(15, 95, 220, 0.8));
  border-radius: 50%;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
  margin-bottom: 16px;
}

.icon {
  font-size: 32px;
  color: #fff;
}

/* Title */
.title {
  font-size: 1.4rem;
  font-weight: 600;
  color: #333;
  margin-bottom: 8px;
}

/* Description */
.description {
  font-size: 1rem;
  color: #555;
  line-height: 1.5;
  margin-bottom: 20px;
  max-width: 280px;
}

/* Button */
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12px 24px;
  font-size: 1rem;
  font-weight: 500;
  color: #fff;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  border-radius: 50px;
  text-decoration: none;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
}

.button:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.25);
}

/* Arrow Animation */
.arrow {
  margin-left: 10px;
  transition: transform 0.3s ease;
}

.button:hover .arrow {
  transform: translateX(6px);
}





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
