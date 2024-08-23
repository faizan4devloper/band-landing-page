// src/components/Footer/Footer.js
import React from 'react';
import styles from './Footer.module.css';

const Footer = () => {
  return (
    <footer className={styles.footer}>
      <div className={styles.blogSection}>
        <h3 className={styles.blogTitle}>Latest Blogs</h3>
        <ul className={styles.blogList}>
          <li><a href="/blog/1" className={styles.blogLink}>How to Optimize Your React App</a></li>
          <li><a href="/blog/2" className={styles.blogLink}>Understanding CSS Grid in 2024</a></li>
          <li><a href="/blog/3" className={styles.blogLink}>AWS Services for Beginners</a></li>
        </ul>
      </div>
      <div className={styles.footerInfo}>
        <p>© 2024 Your Company. All rights reserved.</p>
        <p>Contact: info@yourcompany.com</p>
      </div>
    </footer>
  );
};

export default Footer;


/* src/components/Footer/Footer.module.css */
.footer {
  background-color: #f8f8f8;
  padding: 20px;
  text-align: center;
  border-top: 1px solid #ddd;
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
  color: #333;
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
