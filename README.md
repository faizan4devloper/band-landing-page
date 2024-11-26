// NewClaimPage.js
import React from "react";
import Sidebar from "./Sidebar";
import MainContent from "./MainContent";
import styles from "./NewClaimPage.module.css";

const NewClaimPage = () => {
  return (
    <div className={styles.newClaimPage}>
      <Sidebar />
      <MainContent />
    </div>
  );
};

export default NewClaimPage;



.sidebar {
  width: 20%;
  background-color: #f5f5f5;
  padding: 1rem;
}


.mainContent {
  flex: 1;
  padding: 2rem;
  background-color: #ffffff;
}



.newClaimPage {
  display: flex;
  height: 100vh; /* Adjust to fit the full viewport height */
}
