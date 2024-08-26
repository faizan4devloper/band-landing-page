import React from 'react';
import styles from './Footer.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faEnvelopeOpenText, faBolt, faUserTie } from "@fortawesome/free-solid-svg-icons";

const Footer = () => {
  return (
    <footer className={styles.footer}>
      <div className={styles.footerHeadingContainer}>
        <h2 className={styles.footerHeading}>Explore Our Blogs</h2>
      </div>
      <div className={styles.cardContainer}>
        <div className={styles.card}>
          <div className={styles.iconContainer}>
            <FontAwesomeIcon icon={faEnvelopeOpenText} className={styles.cardIcon} />
          </div>
          <h3 className={styles.cardTitle}>Generative AI-powered email EAR</h3>
          <p className={styles.cardDescription}>Extract, act, and respond on AWS</p>
          <a href="https://www.hcltech.com/blogs/generative-ai-powered-email-ear-on-aws" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            Read The Blog <FontAwesomeIcon icon={faArrowRight} />
          </a>
        </div>
        <div className={styles.card}>
          <div className={styles.iconContainer}>
            <FontAwesomeIcon icon={faBolt} className={styles.cardIcon} />
          </div>
          <h3 className={styles.cardTitle}>LLM cache</h3>
          <p className={styles.cardDescription}>Sustainable, fast, cost-effective GenAI app design</p>
          <a href="https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            Read The Blog <FontAwesomeIcon icon={faArrowRight} />
          </a>
        </div>
        <div className={styles.card}>
          <div className={styles.iconContainer}>
            <FontAwesomeIcon icon={faUserTie} className={styles.cardIcon} />
          </div>
          <h3 className={styles.cardTitle}>Future of recruitment with Smart Recruit</h3>
          <p className={styles.cardDescription}>Streamlining the recruitment process using AI</p>
          <a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            Read The Blog <FontAwesomeIcon icon={faArrowRight} />
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
  border-top: 0.1px solid rgba(219, 197, 255, 1);
  padding: 40px 20px;
  text-align: center;
  color: #fcfcfc;
  margin: 30px -88px;
  margin-bottom: 0px;
  position: relative;
  overflow: hidden;
}

.footerHeadingContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 40px;
}

.footerHeading {
  font-size: 32px; /* Larger font for more emphasis */
  color: #fff;
  letter-spacing: 2px;
  background: linear-gradient(to right, #6f36cd, #1f77f6); /* Gradient background */
  padding: 10px 20px; /* Padding for better appearance */
  border-radius: 8px; /* Rounded corners for a modern look */
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2); /* Subtle shadow for depth */
}

.cardContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 30px;
}

.card {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  border-radius: 12px;
  padding: 20px 15px;
  width: 260px; /* Increased width for better content fit */
  color: #fcfcfc;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  text-align: left;
  position: relative;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 30px rgba(0, 0, 0, 0.3);
}

.iconContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 15px;
}

.cardIcon {
  font-size: 40px; /* Larger icon size */
  color: #fff;
}

.cardTitle {
  font-size: 20px; /* Increased font size */
  font-weight: bold; /* Bold for emphasis */
  margin-bottom: 10px;
  color: #fff;
  line-height: 1.4;
}

.cardDescription {
  font-size: 16px; /* Increased font size for better readability */
  font-weight: 500; /* Semi-bold for better emphasis */
  color: #f0f0f0;
  margin-bottom: 20px;
}

.cardLink {
  margin-top: auto;
  color: #fff;
  text-decoration: none;
  font-weight: bold;
  background-color: #ff80bf; /* Added background color */
  padding: 8px 12px; /* Padding for better button appearance */
  border-radius: 5px; /* Rounded corners for button */
  transition: background-color 0.3s ease, transform 0.3s ease, color 0.3s ease;
  display: inline-flex;
  align-items: center;
  gap: 5px;
  font-size: 16px; /* Increased font size */
}

.cardLink:hover {
  background-color: #ff4d94; /* Darker color on hover */
  color: #fff;
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
