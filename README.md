import React, { forwardRef } from "react";
import styles from "./FeatureCard.module.css";

const FeatureCard = forwardRef(({ icon, title, description }, ref) => {
  return (
    <div ref={ref} className={styles.card}>
      <div className={styles.icon}>{icon}</div>
      <h3>{title}</h3>
      <p>{description}</p>
    </div>
  );
});

export default FeatureCard;
