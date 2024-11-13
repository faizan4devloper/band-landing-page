<section className={styles.section}>
  <h3>Resume Summary</h3>
  {data.resume_summary ? (
    <p className={styles.resumeSummary}>{data.resume_summary}</p>
  ) : (
    <p className={styles.noData}>No resume summary available</p>
  )}
</section>



/* Styling for the main content area */
.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  overflow-y: auto;
}

/* Styling for each section */
.section {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  height: 280px;
  overflow-y: auto;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

/* Hover effect for the section */
.section:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

/* Header styling (for section titles like "Resume Summary") */
h3 {
  margin-bottom: 15px;
  font-size: 1.2rem;
  color: #333;
}

/* Styling for lists (if needed) */
ul {
  padding-left: 20px;
  list-style-type: disc;
}

li {
  font-size: 0.7rem;
  color: #666;
  margin-bottom: 8px;
}

/* Links styling */
a {
  color: #0073e6;
  text-decoration: none;
  transition: color 0.3s;
}

a:hover {
  color: #005bb5;
}

/* New styling for Resume Summary text */
.resumeSummary {
  font-size: 1rem;
  color: #333;
  line-height: 1.6;
  padding: 10px;
  background-color: #f1f1f1;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  overflow-y: auto;
  max-height: 200px;
}

.resumeSummary:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
  transform: translateY(-2px);
}

/* Styling for the case when no resume summary is available */
.noData {
  font-size: 1rem;
  color: #999;
  padding: 10px;
  background-color: #f1f1f1;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
