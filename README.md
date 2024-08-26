/* Card Container */
.cardContainer {
  position: relative;
  height: 800px; /* Adjust the height as needed */
  overflow: hidden; /* Hide overflowing cards */
  padding: 100px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Individual Card */
.card {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  background: linear-gradient(to bottom right, #cb34eb, #7acdf1); /* Gradient background */
  border-radius: 15px;
  padding: 30px;
  color: #fff;
  width: 100%;
  max-width: 600px;
  box-shadow: 0px 10px 15px rgba(0, 0, 0, 0.2);
  transform: translateY(100px); /* Initial stacking effect */
  opacity: 0;
  transition: transform 0.5s ease, opacity 0.5s ease;
  pointer-events: none; /* Prevent interaction with hidden cards */
}

/* Reveal Effect */
.card.visible {
  transform: translateY(0);
  opacity: 1;
  pointer-events: auto; /* Enable interaction once visible */
}

/* Text Styling */
.cardTitle {
  font-size: 1.5em;
  font-weight: bold;
  margin: 0;
}

.card-link {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: inherit;
  font-weight: bold;
}

.card-link svg {
  margin-left: 10px;
  font-size: 1.5em;
}



import React, { useEffect, useRef } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons';
import styles from './CaseStudyCards.module.css';

const CaseStudyCard = ({ title, link }) => (
  <div className={styles.card}>
    <div className={styles.cardTitle}>{title}</div>
    <a href={link} className={styles.cardLink}>
      Read the Case Study <FontAwesomeIcon icon={faArrowRight} />
    </a>
  </div>
);

const CaseStudySection = () => {
  const cardsRef = useRef([]);

  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add(styles.visible);
            observer.unobserve(entry.target);
          }
        });
      },
      { threshold: 0.2 } // Adjust this threshold as needed
    );

    cardsRef.current.forEach((card) => {
      if (card) {
        observer.observe(card);
      }
    });

    return () => observer.disconnect();
  }, []);

  const caseStudies = [
    { title: 'Volkswagen Group connects data across 124 factories', link: 'https://example.com/case-study-1' },
    { title: 'Bayer Crop Science enables sustainable agriculture for farmers', link: 'https://example.com/case-study-2' },
    // Add more case studies as needed
  ];

  return (
    <div className={styles.cardContainer}>
      {caseStudies.map((study, index) => (
        <div
          key={index}
          className={styles.card}
          ref={(el) => (cardsRef.current[index] = el)}
        >
          <CaseStudyCard title={study.title} link={study.link} />
        </div>
      ))}
    </div>
  );
};

export default CaseStudySection;
