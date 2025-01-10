/* Main container with rounded corners and subtle shadow */
.dashboardTableContainer {
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin-bottom: 20px;
}

/* Header with spacing and alignment */
.tableHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

/* Refresh button with modern look */
.refreshButton {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.refreshButton:hover {
  background-color: #45a049;
  transform: scale(1.05);
}

.refreshButton:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

/* Table styling with zebra stripes */
.documentTable {
  width: 100%;
  border-collapse: collapse;
}

.documentTable th, .documentTable td {
  border: 1px solid #ddd;
  padding: 12px;
  text-align: left;
}

.documentTable th {
  background-color: #f9f9f9;
  font-weight: bold;
}

.documentTable tr:nth-child(even) {
  background-color: #f2f2f2;
}

.documentTable tr:hover {
  background-color: #f1f1f1;
}

/* Action buttons for view and download */
.actionButtons {
  display: flex;
  gap: 10px;
}

.viewButton, .downloadButton {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.viewButton:hover {
  background-color: #0056b3;
}

.downloadButton {
  background-color: #28a745;
}

.downloadButton:hover {
  background-color: #218838;
}

/* Pagination container */
.pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  gap: 10px;
}

.pagination button {
  padding: 8px 16px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.pagination button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.pagination button:hover:not(:disabled) {
  background-color: #0056b3;
}

.pagination span {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px 12px;
  background-color: #f9f9f9;
  border-radius: 4px;
  font-weight: bold;
}





<div className={styles.pagination}>
  <motion.button 
    onClick={() => handlePageChange('prev')}
    disabled={currentPage === 0}
    whileHover={{ scale: 1.1 }}
    whileTap={{ scale: 0.95 }}
  >
    Previous
  </motion.button>
  <span>Page {currentPage + 1}</span>
  <motion.button 
    onClick={() => handlePageChange('next')}
    disabled={!nextStartKey}
    whileHover={{ scale: 1.1 }}
    whileTap={{ scale: 0.95 }}
  >
    Next
  </motion.button>
</div>

<div className={styles.actionButtons}>
  <motion.button 
    className={styles.viewButton}
    whileHover={{ scale: 1.1 }}
    whileTap={{ scale: 0.95 }}
    title="View Document"
  >
    <FontAwesomeIcon icon={faEye} />
  </motion.button>
  <motion.button 
    className={styles.downloadButton}
    whileHover={{ scale: 1.1 }}
    whileTap={{ scale: 0.95 }}
    title="Download Document"
  >
    <FontAwesomeIcon icon={faDownload} />
  </motion.button>
</div>
