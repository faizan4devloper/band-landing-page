<section className={styles.section}>
  <h3>Skills</h3>
  {data.skills ? (
    <ul className={styles.skillList}>
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
  padding-left: 0;
  list-style-type: none;
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.skillList {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.skillItem {
  background-color: #0073e6;
  color: #fff;
  padding: 6px 12px;
  border-radius: 4px;
  font-size: 0.8rem;
  font-weight: 500;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.skillItem:hover {
  background-color: #005bb5;
  transform: scale(1.05);
}

a {
  color: #0073e6;
  text-decoration: none;
  transition: color 0.3s;
}

a:hover {
  color: #005bb5;
}

.jobUrlList {
  list-style-type: decimal;
  padding-left: 20px;
  margin: 0;
}

.jobUrlList a {
  color: #0073e6;
  text-decoration: none;
  font-weight: bold;
  transition: color 0.3s;
}

.jobUrlList a:hover {
  color: #005bb5;
}
