.mainContent {
  flex-grow: 1;
  height: 100vh;
  padding: 35px 0;
  background-color: #f7f9fc;
  overflow-y: auto;
  width: 800px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}

ul {
  padding-left: 20px;
}

li {
  font-size: 1em;
  color: #444;
}






.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  max-height: 300px;
  overflow-y: auto;
  margin-top: 10px;
  gap: 20px;
  background-color: #eff0f7;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.answerList {
  padding-left: 20px;
  list-style: none;
  font-size: 1em;
  color: #555;
}

.answerList li {
  margin-bottom: 10px;
  line-height: 1.6;
  font-size: 0.9rem;
  font-weight: 500;
  color: #333;
}

.gridItem {
  padding: 25px 10px;
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
  text-align: center;
}

.gridItem h3 {
  font-size: 1.4em;
  color: #2c3e50;
  font-weight: 600;
  margin-bottom: 10px;
}

.gridItem:hover {
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

/* Scrollbars for gridAnswer */
.gridAnswer::-webkit-scrollbar, .gridItem::-webkit-scrollbar {
  width: 8px;
}

.gridAnswer::-webkit-scrollbar-track, .gridItem::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.gridAnswer::-webkit-scrollbar-thumb, .gridItem::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 8px;
  border: 2px solid #f1f1f1;
}

.gridAnswer::-webkit-scrollbar-thumb:hover, .gridItem::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}





.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}




.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
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
  font-size: 1rem;
  font-weight: bold;
  padding: 14px;
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
  font-size: 1rem;
  color: #888;
  margin-left: 10px;
  transition: transform 0.3s ease, color 0.3s ease;
}
