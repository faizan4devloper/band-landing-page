import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons';
import styles from './CaseStudyCards.module.css';

const CaseStudyCard = ({ title, link }) => (
  <div className={styles.card}>
    <div className={styles.cardTitle}>{title}</div>
    <a href={link} className="card-link">
      Read the Case Study <FontAwesomeIcon icon={faArrowRight}/>
    </a>
  </div>
);

const CaseStudySection = () => {
  const caseStudies = [
    { title: 'Volkswagen Group connects data across 124 factories', link: 'https://example.com/case-study-1' },
    { title: 'Bayer Crop Science enables sustainable agriculture for farmers', link: 'https://example.com/case-study-2' },
    // Add more case studies as needed
  ];

  return (
    <div className={styles.cardContainer}>
      {caseStudies.map((study, index) => (
        <CaseStudyCard key={index} title={study.title} link={study.link} />
      ))}
    </div>
  );
};

export default CaseStudySection;

/* Card Container */
.cardContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  padding: 20px;
  margin: 0 auto;
  max-width: 800px; /* Adjust to your preference */
}

/* Individual Card */
.card {
  background: linear-gradient(to bottom right, #cb34eb, #7acdf1); /* Gradient background */
  border-radius: 15px;
  padding: 30px;
  color: #fff;
  width: 100%;
  max-width: 600px; /* Adjust to your preference */
  box-shadow: 0px 10px 15px rgba(0, 0, 0, 0.2); /* Shadow for depth */
  display: flex;
  align-items: center;
  justify-content: space-between;
  transition: transform 0.3s ease;
}

/* Card Hover Effect */
.card:hover {
  transform: translateY(-10px); /* Slight lift on hover */
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
