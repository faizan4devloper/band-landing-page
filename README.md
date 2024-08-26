.footer {
  background: linear-gradient(to bottom, #1a1a2e, #16213e);
  border-top: 0.1px solid rgba(219, 197, 255, 1);
  padding: 40px 20px; /* Increased padding for better spacing */
  text-align: center;
  color: #fcfcfc;
  margin: 30px -88px;
  margin-bottom: 0px;
  position: relative;
  overflow: hidden;
}

.footerHeading {
  font-size: 28px; /* Slightly larger font for the heading */
  margin-bottom: 40px;
  color: #fff;
  letter-spacing: 2px;
  border-bottom: 2px solid #fff;
  padding-bottom: 10px; /* Increased padding for better visual separation */
}

.cardContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 30px; /* Increased gap for better spacing */
}

.card {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  border-radius: 12px;
  padding: 20px 15px; /* Adjusted padding for consistency */
  width: 240px; /* Slightly wider for better content fit */
  color: #fcfcfc;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column; /* Ensure elements stack vertically */
  justify-content: space-between; /* Evenly distribute elements */
  text-align: left; /* Left-align text for better readability */
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 30px rgba(0, 0, 0, 0.3);
}

.cardTitle {
  font-size: 18px; /* Slightly larger font for better emphasis */
  margin-bottom: 10px;
  color: #fff;
  line-height: 1.4; /* Improved line height for readability */
}

.card p {
  font-size: 15px; /* Slightly larger font for better readability */
  color: #e0d4d4;
  margin-bottom: 20px; /* Space between description and link */
}

.cardLink {
  margin-top: auto; /* Push the link to the bottom */
  color: #fff;
  text-decoration: none;
  font-weight: bold;
  transition: color 0.3s ease, transform 0.3s ease;
  display: inline-flex;
  align-items: center; /* Align the icon with the text */
  gap: 5px; /* Space between text and icon */
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





import React from 'react';
import styles from './Footer.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Footer = () => {
  return (
    <footer className={styles.footer}>
      <h2 className={styles.footerHeading}>Explore Our Blogs</h2>
      <div className={styles.cardContainer}>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>Generative AI-powered email EAR</h3>
          <p>Extract, act, and respond on AWS</p>
          <a href="https://www.hcltech.com/blogs/generative-ai-powered-email-ear-on-aws" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            Visit Blog <FontAwesomeIcon icon={faArrowRight}/>
          </a>
        </div>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>LLM cache</h3>
          <p>Sustainable, fast, cost-effective GenAI app design</p>
          <a href="https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            Visit Blog <FontAwesomeIcon icon={faArrowRight}/>
          </a>
        </div>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>Future of recruitment with Smart Recruit</h3>
          <p>Streamlining the recruitment process using AI</p>
          <a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            Visit Blog <FontAwesomeIcon icon={faArrowRight}/>
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
