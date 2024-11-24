/* Container for the table */
.tableContainer {
  margin-top: 20px;
  overflow-x: auto;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

/* Table styling */
.table {
  width: 100%;
  border-collapse: collapse;
  font-family: 'Roboto', sans-serif;
  font-size: 16px;
}

/* Header styling */
.table th {
  background-color: #4CAF50; /* Green header */
  color: white;
  text-align: left;
  padding: 12px;
  font-weight: bold;
}

/* Row styling */
.table td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: left;
}

/* Alternate row color for zebra effect */
.table tr:nth-child(even) {
  background-color: #f9f9f9;
}

/* Hover effect */
.table tr:hover {
  background-color: #f1f1f1;
  transition: background-color 0.3s;
}

/* No data styling */
.noData {
  text-align: center;
  color: #666;
  padding: 20px;
  font-style: italic;
}

/* Reload button styling */
.reloadButton {
  padding: 8px 15px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s ease;
}

.reloadButton:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}
