.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding:0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.mainContent h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
  border-bottom: 2px solid rgba(95, 30, 193, 0.8);
  padding-bottom: 5px;
}

.mainContent ul {
  list-style-type: disc;
  margin-left: 20px;
  padding-left: 20px;
}

.mainContent ul li {
  font-size: 14px;
  line-height: 1.6;
  color: #000;
  margin-bottom: 10px;
}

.mainContent img {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 20px 0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.benefits, .description {
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.benefits p, .description ul {
  margin: 0;
  font-size: 14px;
}

.benefits h2, .description h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
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
  const renderIndustryAdoption = () => {
    if (!content.industryAdoption) return null;

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
    industryAdoption: renderIndustryAdoption()
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
  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  const descriptionPoints = content.description.split(". ").map((point, index) => (
    <li key={index}>{point.trim()}</li>
  ));
  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <li key={index}>{point.trim()}</li>
  ));

  const renderIndustryAdoption = () => {
    if (!content.industryAdoption) return null;

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
    industryAdoption: renderIndustryAdoption(),
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
    </div>
  );
};

export default MainContent;




.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
}
