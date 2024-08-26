import React from 'react';
import styles from './Footer.module.css';

const Footer = () => {
  return (
    <footer className={styles.footer}>
      <div className={styles.cardContainer}>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>Generative AI-powered email EAR</h3>
          <p>Extract, act, and respond on AWS</p>
          <a href="https://www.hcltech.com/blogs/generative-ai-powered-email-ear-on-aws" className={styles.cardLink}>Learn More &rarr;</a>
        </div>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>LLM cache</h3>
          <p>Sustainable, fast, cost-effective GenAI app design</p>
          <a href="https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design" className={styles.cardLink}>Learn More &rarr;</a>
        </div>
        <div className={styles.card}>
          <h3 className={styles.cardTitle}>Unlocking the future of recruitment</h3>
          <p>With SmartRecruit</p>
          <a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" className={styles.cardLink}>Learn More &rarr;</a>
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
  width: 250px;
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
  margin-top: 15px;
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













// src/components/Footer/Footer.js
import React from 'react';
import styles from './Footer.module.css';

const Footer = () => {
  return (
    <footer className={styles.footer}>
      <div className={styles.blogSection}>
        <h3 className={styles.blogTitle}>Latest Blogs</h3>
        <ul className={styles.blogList}>
          <li><a href="https://www.hcltech.com/blogs/generative-ai-powered-email-ear-on-aws" className={styles.blogLink}>Generative AI-powered email EAR (extract, act and respond) on AWS</a></li>
          <li><a href="https://www.hcltech.com/blogs/llm-cache-sustainable-fast-cost-effective-genai-app-design" className={styles.blogLink}>LLM cache: Sustainable, fast, cost-effective GenAI app design</a></li>
          <li><a href="https://www.hcltech.com/blogs/unlocking-the-future-of-recruitment-with-smartrecruit" className={styles.blogLink}>Unlocking the future of recruitment with SmartRecruit</a></li>
          
        </ul>
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
  animation: fadeIn 1s ease-out; /* Animation for list items */
}

.blogLink {
  color: #fcfcfc;
  text-decoration: none;
  transition: color 0.3s ease, transform 0.3s ease; /* Added transform transition */
  display: inline-block;
}

.blogLink:hover {
  color: #5931d5;
  transform: translateY(-3px); /* Lift effect on hover */
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
