import React, { useState } from "react";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";

// Multidimensional array for keywords
const solutionKeywords = {
  solution1: {
    description: ["keyword1", "keyword2", "keyword3"],
    benefits: ["keyword4", "keyword5"],
    adoption: ["keyword6", "keyword7", "keyword8"]
  },
  solution2: {
    description: ["keyword9", "keyword10"],
    benefits: ["keyword11", "keyword12", "keyword13"],
    adoption: ["keyword14", "keyword15"]
  },
  // Add more solutions with their respective keywords
};

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  const highlightKeywords = (text, keywords) => {
    const regex = new RegExp(`\\b(${keywords.join("|")})\\b`, "gi");
    return text.replace(regex, (matched) => `<span class="${styles.highlight}">${matched}</span>`);
  };

  const renderDescription = () => {
    if (!content.description) return null;
    const descriptionPoints = content.description.split(". ").map((point, index) => (
      <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim(), solutionKeywords[activeTab].description) }}></li>
    ));
    return (
      <div className={styles.description}>
        <h2>Description</h2>
        <ul>
          {descriptionPoints}
        </ul>
      </div>
    );
  };

  const renderBenefits = () => {
    if (!content.benefits) return null;
    const benefitsPoints = content.benefits.split(". ").map((point, index) => (
      <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim(), solutionKeywords[activeTab].benefits) }}></li>
    ));
    return (
      <div className={styles.benefits}>
        <h2>Benefits</h2>
        <ul>
          {benefitsPoints}
        </ul>
      </div>
    );
  };

  const renderAdoption = () => {
    if (!content.adoption) return null;
    const adoptionRows = content.adoption.map((row, index) => (
      <tr key={index}>
        <td>{row.industry}</td>
        <td>{row.adoption}</td>
      </tr>
    ));
    return (
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
    );
  };

  const contentMap = {
    description: renderDescription(),
    benefits: renderBenefits(),
    adoption: renderAdoption(),
    // Add other content sections (solutionFlow, demo, techArchitecture) as needed
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
