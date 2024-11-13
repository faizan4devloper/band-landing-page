<section className={styles.section}>
  <h3>Skills</h3>
  {data.skills ? (
    <ul className={styles.skillsList}>
      {data.skills.split("•").map((skill, index) => (
        <li key={index} className={styles.skillItem}>
          {skill.trim()}
        </li>
      ))}
    </ul>
  ) : (
    <p>No skills available</p>
  )}
</section>



/* Styling for the skills list */
.skillsList {
  padding-left: 20px;
  list-style-type: none;
  margin: 0;
}

.skillItem {
  font-size: 1rem;
  color: #333;
  margin-bottom: 8px;
  padding: 5px;
  border-radius: 4px;
  transition: background-color 0.3s, color 0.3s;
  cursor: pointer;
}

.skillItem:hover {
  background-color: #0073e6;
  color: #fff;
}

/* Styling for no data available */
.noData {
  font-size: 1rem;
  color: #999;
  padding: 10px;
  background-color: #f1f1f1;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* Optional: Styling for skills list separator (if needed) */
.skillsList .skillItem:before {
  content: "•";
  color: #0073e6;
  margin-right: 10px;
  font-weight: bold;
}
