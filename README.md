.mainContent {
  flex-grow: 1;
  height: 100vh;
  padding: 35px;
  padding-top: 60px;
  background-color: #f7f9fc;
  overflow-y: auto;
  width: 800px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.questionBlock {
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 10px;
  background-color: #ffffff;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  overflow: hidden;
}

.questionBlock:hover {
  background-color: #f0f0f0;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.question {
  font-size: 1.1em;
  font-weight: bold;
  padding: 18px;
  cursor: pointer;
  color: #2a2a2a;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: color 0.3s ease;
}

.question:hover {
  color: #0073e6;
}

.chevronIcon {
  font-size: 1.2em;
  color: #888;
  margin-left: 10px;
  transition: transform 0.3s ease, color 0.3s ease;
}

.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.answerList {
  padding-left: 20px;
  font-size: 1em;
  color: #555;
}

.answerList li {
  margin-bottom: 10px;
  line-height: 1.8;
}

.gridItem {
  padding: 25px;
  background-color: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  min-height: 80px;
  max-height: 80px;
  overflow: hidden;
  position: relative;
  text-align: center;
}

.gridItem h3 {
  font-size: 1.4em;
  color: #2c3e50;
  font-weight: 600;
  margin-bottom: 10px;
}

.gridItem p {
  font-size: 1em;
  color: #7f8c8d;
  line-height: 1.6;
  margin: 0;
}

.gridItem:hover {
  background-color: #ecf0f1;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  transform: translateY(-3px);
}

.gridItem h3:hover {
  color: #0073e6;
}

.expanded {
  max-height: 250px;
  overflow-y: auto;
}

.collapsed {
  max-height: 80px;
  overflow: hidden;
}

/* Custom scrollbar */
.gridItem::-webkit-scrollbar {
  width: 8px;
}

.gridItem::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.gridItem::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 8px;
  border: 2px solid #f1f1f1;
}

.gridItem::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}

.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}

ul {
  list-style-type: disc;
  padding-left: 20px;
}

li {
  font-size: 1em;
  color: #444;
}

.gridAnswer > .gridItem {
  margin-bottom: 20px;
}

/* Additional hover effect for grid items */
.gridItem:hover {
  background: linear-gradient(135deg, #74ebd5 0%, #acb6e5 100%);
  color: white;
}

.gridItem:hover h3, .gridItem:hover p {
  color: #fff;
}

/* Optional hover effect for better UI feedback when grid item is expanded */
.gridItem.expanded:hover {
  background-color: #fff;
  color: #2a2a2a;
}
