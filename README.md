import React, { useEffect } from 'react';
import styles from './Footer.module.css';

const Footer = () => {
  useEffect(() => {
    const handleScroll = () => {
      const blogCards = document.querySelectorAll(`.${styles.blogCard}`);
      blogCards.forEach((card, index) => {
        const rect = card.getBoundingClientRect();
        if (rect.top < window.innerHeight - 100) {
          card.classList.add(styles.scrollVisible);
        }
      });
    };

    window.addEventListener('scroll', handleScroll);
    handleScroll(); // Check visibility on initial render
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  return (
    <footer className={styles.footer}>
      <div className={styles.blogSection}>
        <h3 className={styles.blogTitle}>Latest Blogs</h3>
        <ul className={styles.blogList}>
          <li className={styles.blogCard}>
            <a href="https://www.hcltech.com/blogs/generative-ai-powered-email-ear-on-aws" className={styles.blogLink}>
              Generative AI-powered email EAR (extract, act and respond) on AWS
            </a>
          </li>
          <li className={styles.blogCard}>
            <a href="https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design" className={styles.blogLink}>
              LLM cache: Sustainable, fast, cost-effective GenAI app design
            </a>
          </li>
          <li className={styles.blogCard}>
            <a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" className={styles.blogLink}>
              Unlocking the future of recruitment with SmartRecruit
            </a>
          </li>
        </ul>
        <div className={styles.learnMoreContainer}>
          <a href="/blog" className={styles.learnMoreLink}>
            Learn About Blog
            <span className={styles.arrowIcon}>→</span>
          </a>
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
  background: linear-gradient(to bottom, #14142b 0%, #05041e 100%) !important;
  padding: 20px;
  text-align: center;
  color: #fcfcfc;
  border-top: 1px solid #ddd;
  margin: 30px -88px;
  margin-bottom: 0px;
  height: 100%;
  position: relative;
  overflow: hidden; /* Ensures that animations don’t overflow */
}

.blogSection {
  margin-bottom: 15px;
  position: relative; /* For stacking effect */
}

.blogTitle {
  font-size: 24px;
  color: #5931d5;
  margin-bottom: 10px;
  transition: color 0.3s ease;
}

.blogTitle:hover {
  color: #fcfcfc; /* Change title color on hover */
}

.blogList {
  list-style: none;
  padding: 0;
  position: relative;
  height: 100px; /* Adjust height as needed */
}

.blogCard {
  background-color: #1e1e2f;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  opacity: 0;
  position: absolute;
  width: 100%;
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.blogLink {
  color: #fcfcfc;
  text-decoration: none;
  transition: color 0.3s ease, transform 0.3s ease;
  display: inline-block;
}

.blogLink:hover {
  color: #5931d5;
  transform: translateY(-3px); /* Lift effect on hover */
}

.learnMoreContainer {
  margin-top: 15px;
}

.learnMoreLink {
  color: #fcfcfc;
  text-decoration: none;
  font-size: 18px;
  transition: color 0.3s ease, transform 0.3s ease;
  display: inline-flex;
  align-items: center;
}

.learnMoreLink:hover {
  color: #5931d5;
}

.arrowIcon {
  margin-left: 5px;
  transition: transform 0.3s ease;
}

.learnMoreLink:hover .arrowIcon {
  transform: translateX(3px); /* Slight arrow movement on hover */
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

/* Keyframes for fading in blog list */
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* Stacking effect */
.blogCard:nth-child(1) {
  z-index: 3;
  transform: translateY(0px);
}

.blogCard:nth-child(2) {
  z-index: 2;
  transform: translateY(20px);
}

.blogCard:nth-child(3) {
  z-index: 1;
  transform: translateY(40px);
}

/* Scroll effect */
.scrollVisible {
  opacity: 1;
  transform: translateY(0px);
}
