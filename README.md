
import React, { useState } from "react";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
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

  const solutionFlowRows = content.solutionFlowText?.map((row, index) => (
    <tr key={index}>
      <td>{row.step}</td>
      <td>{row.description}</td>
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
        {solutionFlowRows && (
          <table className={styles.adoptionTable}>
            <thead>
              <tr>
                <th>Steps</th>
                <th>Description</th>
              </tr>
            </thead>
            <tbody>{solutionFlowRows}</tbody>
          </table>
        )}
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
          <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={() => setMaximizedImage(null)} />
          <img src={maximizedImage} alt="Maximized view" className={styles.maximized} />
        </div>
      )}
    </div>
  );
};

export default MainContent;
