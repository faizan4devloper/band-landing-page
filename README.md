.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  font-size: 16px; /* Improved font size for readability */
  color: #333; /* Text color */
  background-color: #ffffff; /* White background for clean design */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Subtle shadow for table */
}

.table th,
.table td {
  padding: 15px; /* Increased padding for better spacing */
  border: 1px solid #ddd; /* Subtle border */
  text-align: left; /* Align text to the left for consistency */
}

.table th {
  background: linear-gradient(135deg, #4a90e2, #357ae8); /* Gradient background */
  color: #fff; /* White text for contrast */
  font-weight: 600; /* Bolder text */
  text-transform: uppercase; /* Uppercase headers for distinction */
  font-size: 14px; /* Slightly smaller font for headers */
}

.table tr:nth-child(even) {
  background-color: #f8f9fa; /* Light grey for alternate rows */
}

.table tr:hover {
  background-color: #e8f0fe; /* Subtle blue hover effect */
}

.table td {
  font-size: 15px; /* Slightly larger font for data cells */
  line-height: 1.5; /* Improved line spacing for readability */
}

.reloadButton {
  background: none;
  border: none;
  color: #357ae8;
  cursor: pointer;
  margin: 0;
  padding: 6px;
  font-size: 14px;
  transition: color 0.3s ease;
}

.reloadButton:hover {
  color: #1d5dbf; /* Darker blue for hover state */
}

.previewButton {
  background-color: #4a90e2;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s ease;
  font-size: 14px; /* Consistent font size */
}

.previewButton:hover {
  background-color: #357ae8; /* Darker blue hover effect */
}

/* Empty Table Message */
.table-empty {
  text-align: center;
  color: #999;
  font-size: 16px;
  padding: 20px;
}

/* Loader Style */
.loader {
  font-style: italic;
  color: #aaa;
}

/* Modal Close Button */
.closeButton {
  background-color: #ff4d4f;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  float: right;
  font-size: 14px;
}

.closeButton:hover {
  background-color: #d9363e;
}
