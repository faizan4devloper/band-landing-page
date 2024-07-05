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
  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <li key={index}>{point.trim()}</li>
  ));

  // Parsing adoption points for table display
  const adoptionRows = content.adoption.split("\n").map((row, index) => {
    const cells = row.split("\t");
    return (
      <tr key={index}>
        {cells.map((cell, i) => (
          <td key={i}>{cell.trim()}</td>
        ))}
      </tr>
    );
  });

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
        <ul>
          {benefitsPoints}
        </ul>
      </div>
    ),
    adoption: (
      <div className={styles.adoption}>
        <h2>Solution Adoption</h2>
        <table className={styles.adoptionTable}>
          <thead>
            <tr>
              <th>Industry</th>
              <th>Solution Adoption</th>
            </tr>
          </thead>
          <tbody>{adoptionRows}</tbody>
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

.description, .benefits, .adoption {
  margin-bottom: 20px;
}

.adoptionTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

.adoptionTable th, .adoptionTable td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.adoptionTable th {
  background-color: #f2f2f2;
  color: #333;
}

.adoptionTable tr:nth-child(even) {
  background-color: #f9f9f9;
}

.adoptionTable tr:hover {
  background-color: #ddd;
}
