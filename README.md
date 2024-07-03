/* eslint-disable jsx-a11y/alt-text */
import React from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight } from "@fortawesome/free-solid-svg-icons";
import styles from "./SideBar.module.css";

// Import individual images for each menu item
import Description from "./Icons/Description.svg";
import SolutionFlow from "./Icons/SolutionFlow.svg";
import Demo from "./Icons/Demo.svg";
import ArchitectureFlow from "./Icons/ArchitectureFlow.svg";
import Benefits from "./Icons/Benefits.svg";

const SideBar = ({ activeTab, handleTabChange }) => {
  const menuItems = [
    { id: "description", label: "Description", img: Description },
    { id: "solutionFlow", label: "Solution Flow", img: SolutionFlow },
    { id: "demo", label: "Demo", img: Demo },
    {
      id: "techArchitecture",
      label: "Technical Architecture",
      img: ArchitectureFlow,
    },
    { id: "benefits", label: "Benefits/Use Cases", img: Benefits },
  ];

  return (
    <nav className={styles.sideBar}>
      {menuItems.map((item) => (
        <button
          key={item.id}
          onClick={() => handleTabChange(item.id)}
          className={`${styles.menuItem} ${
            activeTab === item.id ? styles.active : ""
          }`}
        >
          <img
            src={item.img}
            className={`${styles.iconImg} ${styles.hoverEffect}`}
          />
          <span className={styles.label}>{item.label}</span>
          <FontAwesomeIcon icon={faAngleRight} className={styles.icon} />
        </button>
      ))}
    </nav>
  );
};

export default SideBar;





// src/components/MainContent.js
import React from "react";
import styles from "./MainContent.module.css";
import solutionFlowGraph1 from "./Icons/solutionFlowGraph1.png";
import solutionFlowGraph2 from "./Icons/solutionFlowGraph2.png";
import solutionFlowGraph3 from "./Icons/solutionFlowGraph3.png";
import solutionFlowGraph4 from "./Icons/solutionFlowGraph4.png";
import solutionFlowGraph5 from "./Icons/solutionFlowGraph5.png";
import videoDemoUrl1 from "./Icons/Email-EAR.mp4";
import videoDemoUrl2 from "./Icons/SignatureExtraction.mp4";
import videoDemoUrl3 from "./Icons/IntelligentAssist.mp4";
import videoDemoUrl4 from "./Icons/CaseIntelligent.mp4";
import videoDemoUrl5 from "./Icons/CodeGreat.mp4";
import Video from "./Video";

const MainContent = ({ activeTab, handleTabChange }) => {
  const contentMap = {
    description: (
      <div>
        <h2>Description</h2>
        <p>This is the description content.</p>
      </div>
    ),
    solutionFlow: (
      <div>
        <h2>Solution Flow</h2>
        <img src={solutionFlowGraph1} alt="Solution Flow Graph 1" />
        <img src={solutionFlowGraph2} alt="Solution Flow Graph 2" />
        <img src={solutionFlowGraph3} alt="Solution Flow Graph 3" />
        <img src={solutionFlowGraph4} alt="Solution Flow Graph 4" />
        <img src={solutionFlowGraph5} alt="Solution Flow Graph 5" />
      </div>
    ),
    demo: (
      <div>
        <h2>Demo</h2>
        <Video src={videoDemoUrl1} />
        <Video src={videoDemoUrl2} />
        <Video src={videoDemoUrl3} />
        <Video src={videoDemoUrl4} />
        <Video src={videoDemoUrl5} />
      </div>
    ),
    techArchitecture: (
      <div>
        <h2>Technical Architecture</h2>
        <p>This is the technical architecture content.</p>
      </div>
    ),
    benefits: (
      <div>
        <h2>Benefits/Use Cases</h2>
        <p>This is the benefits and use cases content.</p>
      </div>
    ),
  };

  return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
};

export default MainContent;








import React from "react";
import SideBar from "./SideBar";
import MainContent from "./MainContent";
import styles from "./SideBarPage.module.css";

const SideBarPage = ({ activeTab, setActiveTab }) => {
  const handleTabChange = (tab) => {
    setActiveTab(tab);
  };

  return (
    <div className={styles.sideBarPage}>
      <SideBar activeTab={activeTab} handleTabChange={handleTabChange} />
      <MainContent activeTab={activeTab} />
    </div>
  );
};

export default SideBarPage;

