import React, { useState } from "react";
import styles from "./MainContent.module.css";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  const keywords = [
    "extract", "Act", "Respond", "query", "complaint", "issue", "generates", "user-friendly", "questions", "concerns", "detailed response", "prioritization", "queuing", "delayed responses", "Gen AI-powered", "automating", "reading", "analysis", "thoughtful responding", "customer experience", "automates", "Gen AI-powered", "solution", "organization", "intelligent", "assist", "data capture", "manual processes", "Email EAR", "(Extract, Act and Respond)", "Unified experience"
  ];

  const highlightKeywords = (text) => {
    const regex = new RegExp(`\\b(${keywords.join("|")})\\b`, "gi");
    return text.replace(regex, (matched) => `<span class="${styles.highlight}">${matched}</span>`);
  };

  const descriptionPoints = content.description.split(". ").map((point, index) => (
    <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim()) }}></li>
  ));

  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim()) }}></li>
  ));

  const adoptionRows = content.adoption.map((row, index) => (
    <tr key={index}>
      <td>{row.industry}</td>
      <td>{row.adoption}</td>
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
      <div className={styles.solution}>
        <h2>Solution Flow</h2>
        <img
          src={content.solutionFlow}
          alt="Solution Flow"
          className={maximizedImage === content.solutionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.solutionFlow)}
        />
      </div>
    ),
    demo: (
      <div className={styles.demo}>
        <h2>Demo</h2>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
        <h2>Technical Architecture</h2>
        <img
          src={content.techArchitecture}
          alt="Technical Architecture"
          className={maximizedImage === content.techArchitecture ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.techArchitecture)}
        />
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
      {maximizedImage && (
        <div className={styles.overlay} onClick={() => setMaximizedImage(null)}>
          <img src={maximizedImage} alt="Maximized view" className={styles.maximized} />
        </div>
      )}
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

/* Custom Scrollbar Styling */
.mainContent::-webkit-scrollbar {
  width: 12px; /* Width of the scrollbar */
}

.mainContent::-webkit-scrollbar-track {
  background: #f1f1f1; /* Track background color */
}

.mainContent::-webkit-scrollbar-thumb {
  background-color: #5f1ec1; /* Thumb color */
  border-radius: 20px; /* Rounded corners */
  border: 3px solid #f1f1f1; /* Border around the thumb */
}

.mainContent::-webkit-scrollbar-thumb:hover {
  background-color: #3d1299; /* Thumb color on hover */
}

/* Other existing styles for .mainContent */
.mainContent h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
  margin-top: 0;
  border-bottom: 2px solid rgba(95, 30, 193, 0.8);
  padding-bottom: 5px;
}

.mainContent ul {
  list-style-type: disc;
  margin-left: 20px;
  padding-left: 20px;
}

.mainContent ul li {
  font-size: 12px;
  line-height: 1.6;
  color: #000;
  margin-bottom: 10px;
}

.mainContent img {
  max-width: 90%;
  height: auto;
  display: block;
  margin: 20px auto;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 0.3s ease;
}

.maximized {
  max-width: 90%;
  max-height: 90%;
  margin: auto;
  display: block;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.9);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  cursor: pointer;
}

.benefits, .description, .demo, .architecture, .adoption, .solution {
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 50px;
}

.benefits p, .description li {
  margin: 0;
  font-size: 12px;
}

.benefits h2, .description h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
}

.adoptionTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

.adoptionTable th, .adoptionTable td {
  border: 1px solid #ddd;
  font-size: 12px;
  padding: 10px;
  text-align: left;
  vertical-align: top;
}

.adoptionTable th {
  background-color: #5f1ec1;
  color: #fff;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

.adoptionTable tr:nth-child(even) {
  background-color: #f9f9f9;
}

.adoptionTable tr:hover {
  background-color: #f1f1f1;
}

.adoptionTable td:first-child {
  font-weight: bold;
  color: #5f1ec1;
}

.highlight {
  font-style: italic;
  color: #DD9313;
}
