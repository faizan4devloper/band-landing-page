
import React from "react";
import styles from "./MainContent.module.css";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  // Ensure content object and description are defined
  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  // Splitting the description by periods followed by spaces
  const descriptionPoints = content.description.split(". ").map((point, index) => (
    <li key={index}>{point.trim()}</li>
  ));

  // Splitting the benefits by periods followed by spaces
  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <tr key={index}>
      <td>{index + 1}</td>
      <td>{point.trim()}</td>
    </tr>
  ));

  const contentMap = {
    description: (
      <div className={styles.description}>
        <h2>Description</h2>
        <ul>
          {descriptionPoints}
        </ul>
      </div>
    ),
    solutionFlow: (
      <div>
        <h2>Solution Flow</h2>
        <img src={content.solutionFlow} alt="Solution Flow" />
      </div>
    ),
    demo: (
      <div>
        <h2>Demo</h2>
        <p>{/* Add any additional demo description content here */}</p>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div>
        <h2>Technical Architecture</h2>
        <img src={content.techArchitecture} alt="Technical Architecture" />
      </div>
    ),
    benefits: (
      <div className={styles.benefits}>
        <h2>Benefits</h2>
        <table className={styles.benefitsTable}>
          <thead>
            <tr>
              <th>#</th>
              <th>Benefit</th>
            </tr>
          </thead>
          <tbody>
            {benefitsPoints}
          </tbody>
        </table>
      </div>
    ),
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
    </div>
  );
};

export default MainContent;



.mainContent {
  padding: 20px;
}

.benefits {
  margin-top: 20px;
}

.benefits h2 {
  font-size: 1.5em;
  color: #333;
}

.benefitsTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

.benefitsTable th, .benefitsTable td {
  border: 1px solid #ddd;
  padding: 8px;
}

.benefitsTable th {
  background-color: #f2f2f2;
  text-align: left;
}

.benefitsTable tr:nth-child(even) {
  background-color: #f9f9f9;
}

.benefitsTable tr:hover {
  background-color: #ddd;
}
