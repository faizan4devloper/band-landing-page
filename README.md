/* Updated Table Styling */
.table {
  table-layout: fixed; /* Enforces consistent column widths */
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  font-size: 14px;
}

.table th, .table td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: center;
  word-wrap: break-word; /* Handles long content gracefully */
  overflow: hidden;
}

.table th {
  background: #0f5fdc;
  color: white;
  font-weight: bold;
}

.table tr:nth-child(even) {
  background-color: #f9f9f9;
}

.table tr:hover {
  background-color: #f1f1f1;
}

/* Set specific column widths for better consistency */
.table th:nth-child(1),
.table td:nth-child(1) {
  width: 10%; /* Adjust as per requirement */
}

.table th:nth-child(2),
.table td:nth-child(2) {
  width: 15%;
}

.table th:nth-child(3),
.table td:nth-child(3) {
  width: 20%;
}

.table th:nth-child(4),
.table td:nth-child(4) {
  width: 30%;
}

.table th:nth-child(5),
.table td:nth-child(5) {
  width: 15%;
}

.table th:nth-child(6),
.table td:nth-child(6) {
  width: 10%;
}

/* Adjust Sidebar Styling */
.mainContent {
  display: flex;
  gap: 20px; /* Prevent elements from overlapping */
}

.sidebar {
  flex-basis: 250px; /* Fixed width for the sidebar */
  flex-shrink: 0; /* Prevent shrinking when table grows */
}

.mainContent .tableContainer {
  flex-grow: 1; /* Allow table to take available space */
  overflow-x: auto; /* Add horizontal scroll for narrow screens */
}
