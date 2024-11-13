this type of skills coming from back end in skill section add styling on them

Key Skills: • Data Engineering: SQL, ETL (Extract, Transform, Load), Apache Spark, Hadoop, Airflow • Big Data & Cloud: AWS (Redshift, S3), Google BigQuery, Azure Data Lake • Programming Languages: Python, SQL, Scala • Data Science & Machine Learning: Python, Pandas, Scikit-Learn, Data Visualization • Tools & Frameworks: Jupyter Notebooks, Docker, Git


 <section className={styles.section}>
        <h3>Skills</h3>
        {data.skills ? (
          <p>{data.skills}</p> 
        ) : (
          <p>No skills available</p>
        )}
      </section>



      /* MainContent.module.css */

.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  overflow-y: auto;
}

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

.section:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

h3 {
  margin-bottom: 15px;
  font-size: 1.2rem;
  color: #333;
}

ul {
  padding-left: 20px;
  list-style-type: disc;
}

li {
  font-size: 0.7rem;
  color: #666;
  margin-bottom: 8px;
}

a {
  color: #0073e6;
  text-decoration: none;
  transition: color 0.3s;
}

a:hover {
  color: #005bb5;
}

/* Styling for job URL list */
.jobUrlList {
  list-style-type: decimal;
  padding-left: 20px;
  margin: 0;
}

/* Styles for masked job URL link */
.jobUrlList a {
  color: #0073e6;
  text-decoration: none;
  font-weight: bold;
  transition: color 0.3s;
}

.jobUrlList a:hover {
  color: #005bb5;
}


