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
  border-radius: 8px;
  background-color: #fff;
  transition: background-color 0.3s ease;
  overflow: hidden;
}

.questionBlock:hover {
  background-color: #f1f1f1;
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
  transition: padding 0.3s ease, max-height 0.3s ease; /* Smooth transitions */
  overflow: hidden;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  position: relative;
  min-height: 60px;
  max-height: 60px; /* Initially collapsed height */
}

.gridItem h3 {
  font-size: 1.2em;
  margin-left: 10px;
  margin-bottom: 8px;
  color: #333;
}

.gridItem p {
  font-size: 1em;
  color: #555;
  line-height: 1.6;
  margin: 0;
}

.expanded {
  max-height: 200px; /* Expanded height */
  overflow-y: auto;  /* Scroll when content exceeds max height */
}

.collapsed {
  max-height: 60px; /* Collapsed height */
  overflow: hidden;
}

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

/* Adjust spacing between gridItems inside the gridAnswer */
.gridAnswer > .gridItem {
  margin-bottom: 15px;
}

/* Optional hover effect on gridItem for better UX */
.gridItem:hover {
  background-color: #f9f9f9;
}

/* Optional: Better visual feedback when the gridItem is expanded */
.gridItem.expanded:hover {
  background-color: #fff;
}
