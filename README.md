import React from 'react';
import styles from './Footer.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";



const Footer = () => {
  return (
    <footer className={styles.footer}>
      <div className={styles.cardContainer}>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>Generative AI-powered email EAR</h3>
          <p>Extract, act, and respond on AWS</p>
          <a href="https://www.hcltech.com/blogs/generative-ai-powered-email-ear-on-aws" className={styles.cardLink}>Visit Blogs <FontAwesomeIcon icon={faArrowRight}/></a>
        </div>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>LLM cache</h3>
          <p>Sustainable, fast, cost-effective GenAI app design</p>
          <a href="https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design" className={styles.cardLink}>Visit Blogs <FontAwesomeIcon icon={faArrowRight}/></a>
        </div>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>Unlocking the future of recruitment</h3>
          <p>With SmartRecruit</p>
          <a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" className={styles.cardLink}>Visit Blogs <FontAwesomeIcon icon={faArrowRight}/></a>
        </div>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>Unlocking the future of recruitment</h3>
          <p>With SmartRecruit</p>
          <a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" className={styles.cardLink}>Visit Blogs <FontAwesomeIcon icon={faArrowRight}/></a>
        </div>
      </div>
      <div className={styles.footerInfo}>
        <p>Copyright © 2024 HCL Technologies Limited</p>
        <p>Contact: info@yourcompany.com</p>
      </div>
    </footer>
  );
};

export default Footer;

.footer {
  /*background: linear-gradient(to bottom, #14142b 0%, #05041e 100%) !important;*/
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

.cardContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  padding: 20px 0;
}

.card {
  background: linear-gradient(135deg, #f3ec78, #af4261);
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
  font-size: 18px;
  margin-bottom: 10px;
  color: #ffffff;
}

.cardLink {
  display: block;
  margin-top: 160px;
  color: #f9f9f9;
  text-decoration: none;
  font-weight: bold;
  transition: color 0.3s ease, transform 0.3s ease;
}

.cardLink:hover {
  color: #ffffff;
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
  color: #5931d5; /* Highlight contact info on hover */
}

