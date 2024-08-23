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




/* src/components/Footer/Footer.module.css */
.footer {
  /*background-color: #f8f8f8;*/
  background: linear-gradient(to bottom, #14142b 0, #05041e 100%) !important;
  padding: 20px;
  text-align: center;
  color: #fcfcfc;
  border-top: 1px solid #ddd;
  margin:30px -88px;
  margin-bottom: 0px;
  height: 100%;
}

.blogSection {
  margin-bottom: 15px;
}

.blogTitle {
  font-size: 18px;
  color: #5931d5;
  margin-bottom: 10px;
}

.blogList {
  list-style: none;
  padding: 0;
}

.blogLink {
  color: #fcfcfc;
  text-decoration: none;
  transition: color 0.3s ease;
}

.blogLink:hover {
  color: #5931d5;
}

.footerInfo {
  margin-top: 20px;
  font-size: 14px;
  color: #888;
}
