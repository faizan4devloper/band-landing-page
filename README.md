.mainContent {
  flex-grow: 1;
  padding: 35px;
  padding-top: 60px;
  background-color: #f7f9fc;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.questionBlock {
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
  transition: background-color 0.3s ease;
}

.questionBlock:hover {
  background-color: #f1f1f1;
}

.question {
  font-size: 1.1em;
  padding: 10px;
  cursor: pointer;
  color: #333;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chevronIcon {
  font-size: 1em;
  color: #888;
  transition: transform 0.3s ease;
}

/* Grid styling for the first question */
.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
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
  transition: transform 0.3s ease;
  max-height: 250px; /* Fixed height to limit long content */
  overflow-y: auto; /* Enable scrolling for overflow content */
  position: relative; /* Ensure inner elements are properly positioned */
}

.gridItem:hover {
  transform: translateY(-5px);
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

/* Optional: Add a subtle scrollbar style for better UI */
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

.answer {
  padding: 15px;
  background-color: #f8f9fa;
  font-size: 1em;
  line-height: 1.6;
  color: #555;
}
