.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.mainContent h2 {
  font-size: 24px;
  font-weight: 600;
  color: #333;
  margin-bottom: 15px;
  border-bottom: 2px solid rgba(95, 30, 193, 0.8);
  padding-bottom: 5px;
}

.mainContent p {
  font-size: 14px;
  line-height: 1.6;
  color: #555;
  margin-bottom: 20px;
}

.mainContent img {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 20px 0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.mainContent ol {
  list-style-type: decimal;
  margin-left: 20px;
}

.benefits, .description {
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.benefits p, .description p {
  margin: 0;
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
  const contentMap = {
    description: (
      <div className={styles.description}>
        <h2>Description</h2>
        <p>{content.description}</p>
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
        <p>{content.benefits}</p>
      </div>
    ),
  };

  return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
};

export default MainContent;
