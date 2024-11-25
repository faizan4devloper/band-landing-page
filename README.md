const handleReloadClick = async (recNum) => {
  // Add recNum to loading state
  setLoadingRecNums((prev) => [...prev, recNum]); 
  
  await handleReload(recNum);
  
  // Remove recNum from loading state once done
  setLoadingRecNums((prev) => prev.filter((id) => id !== recNum));
};

return (
  <div className={styles.mainContent}>
    {message && <p className={styles.message}>{message}</p>}

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
              <td>{row.policyid || <span className={styles.loader}>Pending</span>}</td>
              <td>{row.type || <span className={styles.loader}>Pending</span>}</td>
              <td>{row.summary || <span className={styles.loader}>Pending</span>}</td>
              <td>
                {row.previewLink ? (
                  <button
                    className={styles.previewButton}
                    onClick={() => openModal(row.previewLink)}
                  >
                    Preview
                  </button>
                ) : (
                  "Pending"
                )}
              </td>
              <td>
                {loadingRecNums.includes(row.recNum) ? (
                  // Show loader while reloading
                  <span>
                    Reloading... 
                    <FontAwesomeIcon icon={faRotateRight} spin />
                  </span>
                ) : (
                  row.status === "Pending" ? (
                    <span>
                      Pending
                      <button
                        className={styles.reloadButton}
                        onClick={() => handleReloadClick(row.recNum)}
                      >
                        <FontAwesomeIcon icon={faRotateRight} />
                      </button>
                    </span>
                  ) : (
                    "Completed"
                  )
                )}
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

    {/* Modal */}
    <Modal
      isOpen={isModalOpen}
      onRequestClose={closeModal}
      className={styles.modal}
      overlayClassName={styles.modalOverlay}
      ariaHideApp={false}
    >
      <div className={styles.modalContent}>
        <button className={styles.closeButton} onClick={closeModal}>
          Close
        </button>
        {modalContent ? (
          <iframe
            src={modalContent}
            title="Preview"
            className={styles.modalIframe}
          ></iframe>
        ) : (
          <p>No preview available</p>
        )}
      </div>
    </Modal>
  </div>
);
