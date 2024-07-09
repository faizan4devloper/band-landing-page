import React, { useState } from "react";
import Modal from "react-modal";
import styles from "./MainContent.module.css";
import Video from "./Video";

// Required for react-modal to work properly
Modal.setAppElement("#root");

const MainContent = ({ activeTab, content }) => {
  const [isOpen, setIsOpen] = useState(false);
  const [modalImage, setModalImage] = useState("");

  const openModal = (imageSrc) => {
    setModalImage(imageSrc);
    setIsOpen(true);
  };

  const closeModal = () => {
    setIsOpen(false);
    setModalImage("");
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
        <img src={content.solutionFlow} alt="Solution Flow" onClick={() => openModal(content.solutionFlow)} />
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
        <img src={content.techArchitecture} alt="Technical Architecture" onClick={() => openModal(content.techArchitecture)} />
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
      <Modal isOpen={isOpen} onRequestClose={closeModal} className={styles.modal} overlayClassName={styles.overlay}>
        <img src={modalImage} alt="Modal Content" className={styles.modalImage} />
        <button onClick={closeModal} className={styles.closeButton}>Close</button>
      </Modal>
    </div>
  );
};

export default MainContent;


.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  right: auto;
  bottom: auto;
  margin-right: -50%;
  transform: translate(-50%, -50%);
  max-width: 90%;
  max-height: 90%;
  padding: 0;
  border: none;
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
  background-color: white;
}

.overlay {
  background-color: rgba(0, 0, 0, 0.75);
}

.modalImage {
  width: 100%;
  height: auto;
}

.closeButton {
  position: absolute;
  top: 10px;
  right: 10px;
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: white;
}
