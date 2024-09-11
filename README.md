/* eslint-disable jsx-a11y/alt-text */
import React from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight } from "@fortawesome/free-solid-svg-icons";
import styles from "./SideBar.module.css";

// Import individual images for each menu item
import descriptionImg from "./Icons/Description.svg";
import solutionFlowImg from "./Icons/SolutionFlow.svg";
import demoImg from "./Icons/Demo.svg";
import techArchitectureImg from "./Icons/ArchitectureFlow.svg";
import benefitsImg from "./Icons/Benefits.svg";
import industyImg from "./Icons/Industry.svg"

const SideBar = ({ activeTab, handleTabChange }) => {
  const menuItems = [
    { id: "description", label: "Description", img: descriptionImg },
    { id: "solutionFlow", label: "Solution Flow", img: solutionFlowImg },
    { id: "demo", label: "Demo Video", img: demoImg },
    {
      id: "techArchitecture",
      label: "Technical Architecture",
      img: techArchitectureImg,
    },
    { id: "benefits", label: "Benefits", img: benefitsImg },
    { id:"adoption", label: "Industry Adoption", img: industyImg  },
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
