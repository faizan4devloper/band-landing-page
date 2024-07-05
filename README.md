/* MainContent.module.css */

.mainContent {
  padding: 20px;
}

.description,
.benefits {
  margin-bottom: 20px;
}

.industryAdoption {
  margin-bottom: 20px;
}

.industryAdoption table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #ddd;
}

.industryAdoption th,
.industryAdoption td {
  padding: 8px;
  text-align: left;
  border: 1px solid #ddd;
}

.industryAdoption th {
  background-color: #f2f2f2;
  font-weight: bold;
}




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

  // Function to render industry adoption table
  const renderIndustryAdoptionTable = () => {
    return (
      <div className={styles.industryAdoption}>
        <h2>Industry Adoption</h2>
        <table>
          <thead>
            <tr>
              <th>Industry</th>
              <th>Solution Adoption</th>
            </tr>
          </thead>
          <tbody>
            {content.industryAdoption.map((item, index) => (
              <tr key={index}>
                <td>{item.industry}</td>
                <td>{item.adoption}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    );
  };

  const contentMap = {
    description: (
      <div className={styles.description}>
        <h2>Description</h2>
        <ul>{descriptionPoints}</ul>
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
        <ul>{benefitsPoints}</ul>
      </div>
    ),
    industryAdoption: renderIndustryAdoptionTable()
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
    </div>
  );
};

export default MainContent;





import React from "react";
import styles from "./MainContent.module.css";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  // Ensure content object and description are defined
  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  // Function to render industry adoption table
  const renderIndustryAdoptionTable = () => {
    return (
      <div className={styles.industryAdoption}>
        <h2>Industry Adoption</h2>
        <table className={styles.industryTable}>
          <thead>
            <tr>
              <th>Industry</th>
              <th>Solution Adoption</th>
            </tr>
          </thead>
          <tbody>
            {content.industryAdoption.map((item, index) => (
              <tr key={index}>
                <td>{item.industry}</td>
                <td>{item.adoption}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    );
  };

  // Splitting the description by periods followed by spaces
  const descriptionPoints = content.description.split(". ").map((point, index) => (
    <li key={index}>{point.trim()}</li>
  ));

  // Splitting benefits by periods followed by spaces
  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <li key={index}>{point.trim()}</li>
  ));

  // Map to render different content sections based on activeTab
  const contentMap = {
    description: (
      <div className={styles.description}>
        <h2>Description</h2>
        <ul>{descriptionPoints}</ul>
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
        <ul>{benefitsPoints}</ul>
      </div>
    ),
    industryAdoption: renderIndustryAdoptionTable(),
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
    </div>
  );
};

export default MainContent;




/* MainContent.module.css */

.mainContent {
  padding: 20px;
  font-family: Arial, sans-serif;
}

.description {
  margin-bottom: 20px;
}

.description h2 {
  color: #333;
  font-size: 24px;
  margin-bottom: 10px;
}

.description ul {
  list-style-type: disc;
  padding-left: 20px;
}

.solutionFlow {
  margin-bottom: 20px;
}

.solutionFlow h2 {
  color: #333;
  font-size: 24px;
  margin-bottom: 10px;
}

.demo {
  margin-bottom: 20px;
}

.demo h2 {
  color: #333;
  font-size: 24px;
  margin-bottom: 10px;
}

.techArchitecture {
  margin-bottom: 20px;
}

.techArchitecture h2 {
  color: #333;
  font-size: 24px;
  margin-bottom: 10px;
}

.benefits {
  margin-bottom: 20px;
}

.benefits h2 {
  color: #333;
  font-size: 24px;
  margin-bottom: 10px;
}

.benefits ul {
  list-style-type: disc;
  padding-left: 20px;
}

.industryAdoption {
  margin-bottom: 20px;
}

.industryAdoption h2 {
  color: #333;
  font-size: 24px;
  margin-bottom: 10px;
}

.industryTable {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #ddd;
}

.industryTable th,
.industryTable td {
  padding: 8px;
  text-align: left;
  border: 1px solid #ddd;
}

.industryTable th {
  background-color: #f2f2f2;
  font-weight: bold;
}
