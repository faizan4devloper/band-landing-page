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
          <a href="https://www.hcltech.com/blogs/generative-ai-powered-email-ear-on-aws" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>Visit Blog <FontAwesomeIcon icon={faArrowRight}/></a>
        </div>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>LLM cache</h3>
          <p>Sustainable, fast, cost-effective GenAI app design</p>
          <a href="https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>Visit Blog <FontAwesomeIcon icon={faArrowRight}/></a>
        </div>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>Future of recruitment with Smart Recruit</h3>
          <p>streamlining the recruitment process using AI</p>
          <a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>Visit Blog <FontAwesomeIcon icon={faArrowRight}/></a>
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
  padding: 20px;
  text-align: center;
  color: #fcfcfc;
  margin: 30px -88px;
  margin-bottom: 0px;
  height: 100%;
  position: relative;
  overflow: hidden;
}

.footerHeading {
  font-size: 24px;
  margin-bottom: 30px;
  color: #fff;
  letter-spacing: 2px;
  border-bottom: 2px solid #fff;
  padding-bottom: 5px;
  /*text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4); */
}

.cardContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  padding: 20px 0;
}

.card {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  border-radius: 12px;
  padding: 20px;
  width: 220px;
  color: #fcfcfc;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 30px rgba(0, 0, 0, 0.3);
}

.cardTitle {
  font-size: 16px;
  margin-bottom: 15px;
  color: #fff;
  /*text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);*/
}

.card p {
  font-size: 14px;
  color: #ddcece;
}

.cardLink {
  display: block;
  margin-top: 20px;
  color: #fff;
  text-decoration: none;
  font-weight: bold;
  transition: color 0.3s ease, transform 0.3s ease;
}

.cardLink:hover {
  color: #ff80bf;
  transform: translateX(5px);
}

.footerInfo {
  margin-top: 20px;
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








