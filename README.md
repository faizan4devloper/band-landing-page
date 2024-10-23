.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* Independent columns */
  gap: 20px;
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.gridItem {
  padding: 20px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: height 0.3s ease, padding 0.3s ease;
  overflow: hidden;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.gridItem h3 {
  font-size: 1.2em;
  margin-bottom: 10px;
  color: #333;
}

.gridItem p {
  font-size: 1em;
  color: #555;
  line-height: 1.6;
}

.expanded {
  height: auto; /* Allow it to expand based on content */
}

.collapsed {
  height: 60px; /* Collapsed height */
  overflow: hidden;
  padding: 20px;
}

.gridItem.expanded {
  height: auto; /* Let the item expand as much as needed */
}

.gridItem.collapsed {
  height: 60px; /* Keep collapsed to a fixed height */
}

/* Optional: Subtle scrollbar for better UX */
.gridItem::-webkit-scrollbar {
  width: 8px;
}

.gridItem::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 8px;
}

.gridItem::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 8px;
  border: 2px solid #f1f1f1;
}

.gridItem::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}




<div
  className={`${styles.gridItem} ${
    openGridItems.textualResponse ? styles.expanded : styles.collapsed
  }`}
  onClick={() => toggleGridItem('textualResponse')}
>
  <h3>Textual Response</h3>
  {openGridItems.textualResponse && <p>{item.answer.textualResponse}</p>}
</div>

<div
  className={`${styles.gridItem} ${
    openGridItems.citizenExperience ? styles.expanded : styles.collapsed
  }`}
  onClick={() => toggleGridItem('citizenExperience')}
>
  <h3>Citizen Experience</h3>
  {openGridItems.citizenExperience && <p>{item.answer.citizenExperience}</p>}
</div>

<div
  className={`${styles.gridItem} ${
    openGridItems.factualInfo ? styles.expanded : styles.collapsed
  }`}
  onClick={() => toggleGridItem('factualInfo')}
>
  <h3>Factual Info</h3>
  {openGridItems.factualInfo && <p>{item.answer.factualInfo}</p>}
</div>

<div
  className={`${styles.gridItem} ${
    openGridItems.contextual ? styles.expanded : styles.collapsed
  }`}
  onClick={() => toggleGridItem('contextual')}
>
  <h3>Contextual</h3>
  {openGridItems.contextual && <p>{item.answer.contextual}</p>}
</div>
