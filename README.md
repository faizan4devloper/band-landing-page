style the table add enhancement on the table add colors in table coming the dynamic data thats why style well also stle the messages add the loaders and the preview also add beutiful modal after user click on the preview then opens beutiful modal


import React from 'react';
import styles from './MainContent.module.css'; // Custom CSS for MainContent
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight } from '@fortawesome/free-solid-svg-icons';


const MainContent = ({ message, rows, handleReload }) => {
  return (
    <div className={styles.mainContent}>
      {message && <p>{message}</p>}
      
      {/* Table to display uploaded data */}
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
          {rows && rows.length > 0 ? (
            rows.map((row, index) => (
              <tr key={index}>
                <td>{row.recNum}</td>
                <td>{row.policyid || "Loading..."}</td>
                <td>{row.type || "Loading..."}</td>
                <td>{row.summary || "Loading..."}</td>
                <td>
                  {row.previewLink ? (
                    <a href={row.previewLink} target="_blank" rel="noopener noreferrer">
                      Preview
                    </a>
                  ) : (
                    "Pending"
                  )}
                </td>
                <td>
                  {row.status === "Pending" ? (
                    <span>
                      Pending
                      <button
                        className={styles.reloadButton}
                        onClick={() => handleReload(row.recNum)}
                      >
<FontAwesomeIcon icon={faRotateRight} />                      </button>
                    </span>
                  ) : (
                    row.status
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
    </div>
  );
};

export default MainContent;

.mainContent {
  margin-left: 20px;
  flex-grow: 1;
  padding: 20px;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.table th, .table td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: center;
}

.table th {
  background-color: #f2f2f2;
}


.reloadButton {
  background: none;
  border: none;
  color: blue;
  cursor: pointer;
  margin-left: 8px;
  font-size: 16px;
}

.reloadButton:hover {
  color: darkblue;
}



