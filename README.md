import React from 'react';
import styles from './Footer.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faTabletAlt } from "@fortawesome/free-solid-svg-icons"; // Add more icons as needed

const Footer = () => {
  return (
    <footer className={styles.footer}>
      <h2 className={styles.footerHeading}>Explore Our Blogs</h2>
      <div className={styles.cardContainer}>
        <div className={styles.card}>
          <div className={styles.iconContainer}>
            <FontAwesomeIcon icon={faTabletAlt} className={styles.cardIcon} />
          </div>
          <h3 className={styles.cardTitle}>Generative AI-powered email EAR</h3>
          <p className={styles.cardDescription}>
            Extract, act, and respond on AWS
          </p>
          <a href="https://www.hcltech.com/blogs/generative-ai-powered-email-ear-on-aws" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            <FontAwesomeIcon icon={faArrowRight} />
          </a>
        </div>
        <div className={styles.card}>
          <div className={styles.iconContainer}>
            <FontAwesomeIcon icon={faTabletAlt} className={styles.cardIcon} />
          </div>
          <h3 className={styles.cardTitle}>LLM cache</h3>
          <p className={styles.cardDescription}>
            Sustainable, fast, cost-effective GenAI app design
          </p>
          <a href="https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            <FontAwesomeIcon icon={faArrowRight} />
          </a>
        </div>
        <div className={styles.card}>
          <div className={styles.iconContainer}>
            <FontAwesomeIcon icon={faTabletAlt} className={styles.cardIcon} />
          </div>
          <h3 className={styles.cardTitle}>Future of recruitment with Smart Recruit</h3>
          <p className={styles.cardDescription}>
            Streamlining the recruitment process using AI
          </p>
          <a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            <FontAwesomeIcon icon={faArrowRight} />
          </a>
        </div>
      </div>
      <div className={styles.footerInfo}>
        <p>Copyright © 2024 HCL Technologies Limited</p>
        <p>Contact Us: AWSEBUTA@hcltech.com</p>
      </div>
    </footer>
  );
};

export default Footer;



.footer {
  background: linear-gradient(to bottom, #1a1a2e, #16213e);
  padding: 40px 20px;
  text-align: center;
  color: #fcfcfc;
}

.footerHeading {
  font-size: 28px;
  margin-bottom: 40px;
  color: #fff;
}

.cardContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 30px;
}

.card {
  background: #f4f4f4; /* Light background color for card */
  border-radius: 12px;
  padding: 20px;
  width: 280px; /* Adjusted width */
  color: #333;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center; /* Center align content */
  text-align: center; /* Center text */
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
}

.iconContainer {
  background-color: #e0e0e0; /* Circle background color */
  border-radius: 50%;
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}

.cardIcon {
  font-size: 24px;
  color: #6f36cd; /* Icon color */
}

.cardTitle {
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 10px;
}

.cardDescription {
  font-size: 16px;
  margin-bottom: 20px;
  color: #666; /* Description color */
}

.cardLink {
  color: #6f36cd; /* Link color */
  text-decoration: none;
  font-size: 24px; /* Arrow size */
  transition: color 0.3s ease, transform 0.3s ease;
}

.cardLink:hover {
  color: #ff80bf;
  transform: translateX(5px);
}

.footerInfo {
  margin-top: 30px;
  font-size: 14px;
  color: #888;
}

.footerInfo p {
  margin: 5px 0;
  transition: color 0.3s ease;
}

.footerInfo p:hover {
  color: #ff80bf;
}
