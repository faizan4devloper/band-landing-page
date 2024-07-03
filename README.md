
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
