import React, { useState } from "react";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";

// Define keywords for each solution (or fetch dynamically)
const solutionKeywords = {
  solution1: ["keyword1", "keyword2"],
  solution2: ["keyword3", "keyword4"],
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

  const descriptionPoints = content.description.split(". ").map((point, index) => (
    <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim(), solutionKeywords[activeTab]) }}></li>
  ));

  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim(), solutionKeywords[activeTab]) }}></li>
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
    // Include other content sections (solutionFlow, demo, techArchitecture, benefits, adoption)
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
