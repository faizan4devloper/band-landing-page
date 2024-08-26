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
            <span className={styles.arrowOnly}>
              <FontAwesomeIcon icon={faArrowRight} />
            </span>
            <span className={styles.linkText}>Read The Blog</span>
          </a>
        </div>
        <div className={styles.card}>
          <div className={styles.iconContainer}>
            <FontAwesomeIcon icon={faBolt} className={styles.cardIcon} />
          </div>
          <h3 className={styles.cardTitle}>LLM cache</h3>
          <p className={styles.cardDescription}>Sustainable, fast, cost-effective GenAI app design</p>
          <a href="https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            <span className={styles.arrowOnly}>
              <FontAwesomeIcon icon={faArrowRight} />
            </span>
            <span className={styles.linkText}>Read The Blog</span>
          </a>
        </div>
        <div className={styles.card}>
          <div className={styles.iconContainer}>
            <FontAwesomeIcon icon={faUserTie} className={styles.cardIcon} />
          </div>
          <h3 className={styles.cardTitle}>Future of recruitment with Smart Recruit</h3>
          <p className={styles.cardDescription}>Streamlining the recruitment process using AI</p>
          <a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" target="_blank" rel="noopener noreferrer" className={styles.cardLink}>
            <span className={styles.arrowOnly}>
              <FontAwesomeIcon icon={faArrowRight} />
            </span>
            <span className={styles.linkText}>Read The Blog</span>
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

/* Existing styles remain unchanged */

.cardLink {
  margin-top: auto;
  color: #fff;
  text-decoration: none;
  font-weight: bold;
  display: inline-flex;
  align-items: center;
  gap: 5px;
  font-size: 16px;
  overflow: hidden; /* Hide overflow for smooth transition */
  transition: color 0.3s ease;
  position: relative; /* Position relative for pseudo-elements */
}

.arrowOnly {
  display: inline-block;
  transition: transform 0.3s ease;
}

.linkText {
  display: inline-block;
  margin-left: 5px;
  opacity: 0;
  transition: opacity 0.3s ease, transform 0.3s ease;
  transform: translateX(-10px);
}

.cardLink:hover .arrowOnly {
  transform: translateX(5px);
}

.cardLink:hover .linkText {
  opacity: 1;
  transform: translateX(0);
}

/* Hover effect remains unchanged */
.cardLink:hover {
  color: #ff80bf;
}
