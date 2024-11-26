/* MainContent.module.css */

.mainContent {
  display: flex;
  flex-direction: row;
  gap: 20px;
  padding: 20px;
  background: linear-gradient(135deg, #f2f2f2, #e6f7ff);
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.previewSection {
  flex: 1;
  background: #fff;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  overflow: auto;
}

.extractContent {
  flex: 2;
  background: #fff;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.filterContainer {
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.filterLabel {
  font-weight: bold;
  color: #333;
}

.filterDropdown {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 14px;
}

.table {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
}

.table thead th {
  background-color: #007bff;
  color: #fff;
  text-align: left;
  padding: 10px;
}

.table tbody tr:hover {
  background-color: #f9f9f9;
}

.table td,
.table th {
  border: 1px solid #ddd;
  padding: 10px;
}

.previewButton,
.reloadButton {
  padding: 8px 12px;
  font-size: 12px;
  color: #fff;
  background-color: #007bff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.previewButton:hover,
.reloadButton:hover {
  background-color: #0056b3;
}

.spinnerIcon {
  font-size: 14px;
  color: #007bff;
}

.modal {
  max-width: 90%;
  max-height: 90%;
  margin: auto;
  border-radius: 10px;
  overflow: hidden;
  background: #fff;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  position: relative;
}

.modalOverlay {
  background-color: rgba(0, 0, 0, 0.7);
}

.modalContent {
  padding: 20px;
}

.closeButton {
  position: absolute;
  top: 10px;
  right: 10px;
  background: transparent;
  border: none;
  font-size: 16px;
  cursor: pointer;
  color: #333;
}





<div className={styles.mainContent}>
  {/* Left - Preview Section */}
  <div className={styles.previewSection}>
    <h2>Preview Area</h2>
    {/* Add preview content here */}
  </div>

  {/* Right - Extract Content */}
  <div className={styles.extractContent}>
    <div className={styles.filterContainer}>
      <h2>Extract Content</h2>
      <button className={styles.reloadButton}>
        <FontAwesomeIcon icon={faRotateRight} /> Refresh
      </button>
    </div>

    {/* Filter Dropdown */}
    <div className={styles.filterContainer}>
      <label htmlFor="statusFilter" className={styles.filterLabel}>
        Filter by Status:
      </label>
      <select
        id="statusFilter"
        className={styles.filterDropdown}
        value={filter}
        onChange={handleFilterChange}
      >
        <option value="All">All</option>
        <option value="Pending">Pending</option>
        <option value="Completed">Completed</option>
      </select>
    </div>

    {/* Table */}
    <table className={styles.table}>
      <thead>
        <tr>
          <th>RecNum</th>
          <th>Policy ID</th>
          <th>Product Sheet Type</th>
          <th>Summary</th>
          <th>Preview Link</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        {filteredRows.length > 0 ? (
          filteredRows.map((row, index) => (
            <tr key={index}>
              <td>{row.recNum}</td>
              <td>{row.policy_id}</td>
              <td>{row.prod_sheet_type}</td>
              <td>{row.summary}</td>
              <td>
                <button
                  className={styles.previewButton}
                  onClick={() => openModal(row.previewLink)}
                >
                  Preview
                </button>
              </td>
              <td>
                <button
                  className={styles.reloadButton}
                  onClick={() => handleReload(row.recNum)}
                >
                  Reload
                </button>
              </td>
            </tr>
          ))
        ) : (
          <tr>
            <td colSpan="6">No data available</td>
          </tr>
        )}
      </tbody>
    </table>
  </div>
</div>
