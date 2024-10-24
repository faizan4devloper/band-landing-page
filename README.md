.mainContent {
  flex-grow: 1;
  height: 100vh;
  padding: 35px;
  padding-top: 60px;
  background-color: #f5f7fa;
  overflow-y: auto;
  width: 900px;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.questionBlock {
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  overflow: hidden;
}

.questionBlock:hover {
  background-color: #f8f8f8;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.question {
  font-size: 1.2em;
  padding: 15px;
  cursor: pointer;
  color: #333;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chevronIcon {
  font-size: 1em;
  color: #888;
  margin-left: 10px;
  transition: transform 0.3s ease;
}

.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  background-color: #f1f2f6;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.answerList {
  padding-left: 20px;
  font-size: 1em;
  color: #555;
}

.answerList li {
  margin-bottom: 10px;
  line-height: 1.6;
  font-size: 1.1em;
  color: #333;
}

.gridItem {
  padding: 20px;
  background-color: #fefefe;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: padding 0.3s ease, max-height 0.3s ease, background-color 0.3s ease;
}

.gridItem h3 {
  margin: 0;
  font-size: 1.2em;
  font-weight: 600;
  color: #2d2d2d;
}

.gridItem.expanded {
  padding: 30px;
  max-height: 1000px;
  background-color: #f9fafb;
}

.gridItem.collapsed {
  max-height: 0;
  padding: 0 20px;
  overflow: hidden;
}

.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}
