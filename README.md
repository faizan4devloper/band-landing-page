import React from "react";
import styles from "./ManageClaims.module.css";
import AllDataTable from "./AllDataTable";
import { useNavigate } from "react-router-dom";

const ManageClaims = () => {
  const navigate = useNavigate();

  return (
    <div className={styles.manageClaimsContainer}>
      <header className={styles.header}>
        <h1 className={styles.title}>Manage Claims</h1>
        <button
          className={styles.newClaimButton}
          onClick={() => navigate("/new-claim")}
        >
          New Claim Processing
        </button>
      </header>
      <AllDataTable />
    </div>
  );
};

export default ManageClaims;


import React from "react";
import styles from "./NewClaimPage.module.css";

const NewClaimPage = () => {
  return (
    <div className={styles.newClaimPage}>
      <aside className={styles.sidebar}>
        <nav>
          <ul>
            <li>Option 1</li>
            <li>Option 2</li>
            <li>Option 3</li>
          </ul>
        </nav>
      </aside>
      <main className={styles.mainContent}>
        <h2>New Claim Processing</h2>
        {/* Add your form or content here */}
      </main>
    </div>
  );
};

export default NewClaimPage;

