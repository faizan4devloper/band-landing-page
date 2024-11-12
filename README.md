import React from 'react';
import styles from './MainContent.module.css';

const MainContent = () => {
  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <p>Tech skills, certifications, awards, etc.</p>
        <p>Details about Section 1</p>
      </section>
      <section className={styles.section}>
        <p>Top 5 job postings</p>
        <p>Details about Section 2</p>
      </section>
      <section className={styles.section}>
        <p>How other Similar profiles got job opportunities- Recommendation based on Similar Skill Sets</p>
        <p>Details about Section 3</p>
      </section>
      <section className={styles.section}>
        <p>Country Specific Immigration /visa/wp related insights</p>
        <p>Details about Section 4</p>
      </section>
      <section className={styles.section}>
        <p>Cross Skilling – Recommendations for better job reach</p>
        <p>Details about Section 5</p>
      </section>
    </div>
  );
};

export default MainContent;




.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  background-color: #fff;
}

.section {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.section:hover {
  transform: translateY(-5px);
}

h3 {
  margin-bottom: 10px;
  font-size: 1.2rem;
  color: #333;
}

.section p {
  color: #666;
}
