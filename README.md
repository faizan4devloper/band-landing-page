.container {
  display: flex;
  padding: 5px 40px;
  height: 100vh;
  width: 100%;
  overflow: hidden;
}

.container > div {
  margin-right: 20px;
}

.sidebar {
  width: 250px; /* Fixed width */
  min-width: 250px; /* Prevents shrinking below this size */
}

.mainContent {
  flex-grow: 1; /* Takes the remaining space */
  overflow-x: auto; /* Enables horizontal scroll for wide tables */
}

.infoMessage {
  text-align: center;
  margin: 180px;
  font-size: 1.2rem;
  color: #555;
}

.header {
  display: flex;
  justify-content: flex-start;
  margin-bottom: 20px;
}

.newClaimButton {
  background-color: #007bff;
  margin-top: 10px;
  color: #fff;
  padding: 10px 20px;
  border: none;
  position: absolute;
  right: 80px;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.newClaimButton:hover {
  background-color: #0056b3;
}







.mainContent {
  flex-grow: 1;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.dataTableWrapper {
  overflow-x: auto; /* Horizontal scrolling for the table */
  flex-grow: 1; /* Ensures the table expands within the mainContent */
}

.table {
  width: 100%; /* Ensures the table adapts to the wrapper width */
  border-collapse: collapse;
}

.table th, .table td {
  padding: 10px;
  text-align: left;
  border: 1px solid #ddd;
}
