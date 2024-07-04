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
        <ul>
          {content.benefits.split('• ').map((benefit, index) => {
            if (benefit.trim()) {
              return <li key={index}>{benefit.trim()}</li>;
            }
            return null;
          })}
        </ul>
      </div>
    ),
  };

  return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
};

export default MainContent; 




.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 20px;
  background-color: #ffffff;
  padding-top: 0px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.mainContent h2 {
  font-size: 20px;
  margin-top: 0px;
  font-weight: 500;
  margin-bottom: 10px;
}

.mainContent p {
  font-size: 14px;
  line-height: 1.5;
}

.mainContent ol {
  list-style-type: decimal;
  margin-left: 20px;
}

.mainContent img {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 0 auto;
}

.description {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.benefits {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.benefits ul {
  padding-left: 20px;
}

.benefits li {
  margin-bottom: 10px;
}




import React from "react";
import styles from "./SideBarPage.module.css";
import MainContent from "./MainContent";

const SideBarPage = ({ activeTab, content, onBackButtonClick }) => {
  return (
    <div className={styles.sideBarPage}>
      <div className={styles.header2}>
        <button className={styles.backButton} onClick={onBackButtonClick}>
          Back
        </button>
        <h1 className={styles.cardTitle}>{content.title}</h1>
      </div>
      <div className={styles.contentWrapper}>
        <MainContent activeTab={activeTab} content={content} />
      </div>
    </div>
  );
};

export default SideBarPage;




.sideBarPage {
  display: flex;
  margin-top: 70px;
  margin-right: 40px;
  margin-left: 20px; /* Added margin-left to position it a bit to the left side */
  flex-direction: column;
  min-height: 100vh;
  overflow-y: auto; /* Enable vertical scrolling */
  overscroll-behavior: contain; /* Prevent overscrolling */
  scroll-behavior: smooth; /* Enable smooth scrolling */
}

.header2 {
  display: flex;
  align-items: center;
}

.cardTitle {
  font-size: 18px;
  color: rgba(23, 23, 25, 1);
  font-weight: 600;
  margin-bottom: 10px;
  font-family: "Poppins", sans-serif;
}

.contentWrapper {
  display: flex;
  margin-top: 15px;
  flex: 1;
}

.backButtonContainer {
  display: flex;
  align-items: center;
  padding: 10px;
}

.backButton {
  background-color: rgba(230, 235, 245, 1);
  padding: 8px;
  margin-left: 20px;
  margin-bottom: 10px;
  border-radius: 4px;
  width: 40px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  margin-right: 10px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}
