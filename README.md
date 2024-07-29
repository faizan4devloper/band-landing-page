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

  if (!content) {
    return <div className={styles.mainContent}>Content not available</div>;
  }

  const descriptionImage = (
    <div className={styles.description}>
      <h2>Description</h2>
      <img
        src={content.descriptionImage}
        alt="Description"
        className={maximizedImage === content.descriptionImage ? styles.maximized : ""}
        onClick={() => toggleMaximize(content.descriptionImage)}
      />
    </div>
  );

  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <li key={index}>{point.trim()}</li>
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
    description: descriptionImage,
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



function mapAssets(card) {
  return {
    ...card,
    imageUrl: images[card.imageUrl.split('.').pop()],
    content: {
      ...card.content,
      solutionFlow: solutionFlows[card.content.solutionFlow.split('.').pop()],
      demo: videos[card.content.demo.split('.').pop()],
      techArchitecture: architectures[card.content.techArchitecture.split('.').pop()],
      descriptionImage: images[card.content.descriptionImage.split('.').pop()],
    },
  };
}



{
  "title": "Intelligent Assist",
  "descriptionImage": "path/to/descriptionImage.png",
  "benefits": "Benefit text...",
  "adoption": [{ "industry": "Finance", "adoption": "High" }],
  // other fields
}
