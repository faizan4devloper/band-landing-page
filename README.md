// src/components/CaseStudyCards/CaseStudyCards.js
import React, { useState, useEffect } from 'react';
import styles from './CaseStudyCards.module.css';
import { FaArrowRight } from 'react-icons/fa';

const caseStudies = [
  {
    title: 'Case Study 1',
    link: 'https://example.com/case-study-1',
  },
  {
    title: 'Case Study 2',
    link: 'https://example.com/case-study-2',
  },
  {
    title: 'Case Study 3',
    link: 'https://example.com/case-study-3',
  },
  // Add more case studies here
];

const CaseStudyCards = () => {
  const [revealIndex, setRevealIndex] = useState(0);

  const handleScroll = () => {
    const scrollPosition = window.scrollY + window.innerHeight;
    const revealPosition = window.innerHeight / 1.5;
    if (scrollPosition > revealPosition) {
      setRevealIndex((prev) => Math.min(prev + 1, caseStudies.length));
    }
  };

  useEffect(() => {
    window.addEventListener('scroll', handleScroll);
    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  }, []);

  return (
    <div className={styles.caseStudyContainer}>
      {caseStudies.map((caseStudy, index) => (
        <div
          key={index}
          className={`${styles.caseStudyCard} ${index <= revealIndex ? styles.revealed : ''}`}
        >
          <h3 className={styles.caseStudyTitle}>{caseStudy.title}</h3>
          <a href={caseStudy.link} className={styles.caseStudyLink}>
            Read the Case Study <FaArrowRight className={styles.arrowIcon} />
          </a>
        </div>
      ))}
    </div>
  );
};

export default CaseStudyCards;



/* src/components/CaseStudyCards/CaseStudyCards.module.css */
.caseStudyContainer {
  margin-bottom: 15px;
}

.caseStudyCard {
  background-color: #14142b;
  color: #fcfcfc;
  padding: 20px;
  margin: 10px 0;
  transform: translateY(100px);
  opacity: 0;
  transition: all 0.6s ease-in-out;
}

.caseStudyCard.revealed {
  transform: translateY(0);
  opacity: 1;
}

.caseStudyTitle {
  font-size: 18px;
  color: #5931d5;
  margin-bottom: 10px;
}

.caseStudyLink {
  color: #fcfcfc;
  text-decoration: none;
  font-size: 16px;
  display: flex;
  align-items: center;
  transition: color 0.3s ease;
}

.caseStudyLink:hover {
  color: #5931d5;
}

.arrowIcon {
  margin-left: 8px;
}


// src/components/Footer/Footer.js
import React from 'react';
import styles from './Footer.module.css';
import CaseStudyCards from '../CaseStudyCards/CaseStudyCards';

const Footer = () => {
  return (
    <footer className={styles.footer}>
      <div className={styles.caseStudySection}>
        <CaseStudyCards />
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
  background: linear-gradient(to bottom, #14142b 0, #05041e 100%) !important;
  padding: 20px;
  text-align: center;
  color: #fcfcfc;
  border-top: 1px solid #ddd;
  margin: 30px -88px;
  margin-bottom: 0px;
  height: 100%;
}

.caseStudySection {
  margin-bottom: 15px;
}

.footerInfo {
  margin-top: 20px;
  font-size: 14px;
  color: #888;
}
