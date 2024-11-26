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

