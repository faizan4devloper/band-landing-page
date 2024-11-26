import React, { useEffect, useState } from "react";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faSpinner, faRotateRight } from "@fortawesome/free-solid-svg-icons";
import { v4 as uuidv4 } from "uuid"; // For generating unique IDs

const MainContent = ({ fetchedData }) => {
  const [rows, setRows] = useState([]);
  const [loadingRows, setLoadingRows] = useState([]);
  const [filterText, setFilterText] = useState("");

  // Initialize rows with unique IDs
  useEffect(() => {
    const initialData = fetchedData.map((row) => ({
      id: row.policy_id || uuidv4(), // Use unique ID from API or generate one
      policyid: row.policyid || null,
      prod_sheet_type: row.prod_sheet_type || null,
      summary: row.summary || null,
      status: row.status || null,
      previewLink: row.previewLink || null,
    }));
    setRows(initialData);
  }, [fetchedData]);

  const handleReload = async (id) => {
    try {
      setLoadingRows((prev) => [...prev, id]); // Add ID to loading state
      const singleClaimData = await fetchData(id); // Replace fetchData with your API call
      console.log("Single Claim Data:", singleClaimData);

      setRows((prevRows) =>
        prevRows.map((row) =>
          row.id === id
            ? {
                ...row,
                policyid: singleClaimData.policy_id,
                prod_sheet_type: singleClaimData.prod_sheet_type,
                summary: singleClaimData.summary,
                status: singleClaimData.status,
                previewLink: singleClaimData.previewLink,
              }
            : row
        )
      );
    } catch (error) {
      console.error("Error reloading row data:", error);
    } finally {
      setLoadingRows((prev) => prev.filter((rowId) => rowId !== id)); // Remove from loading state
    }
  };

  const filteredRows = rows.filter((row) =>
    row.policyid?.toLowerCase().includes(filterText.toLowerCase())
  );

  const openModal = (link) => {
    // Handle modal opening for the preview link
    console.log("Opening modal for link:", link);
  };

  return (
    <div className={styles.container}>
      <input
        type="text"
        placeholder="Filter by policy ID..."
        className={styles.filterInput}
        value={filterText}
        onChange={(e) => setFilterText(e.target.value)}
      />
      <table className={styles.table}>
        <thead>
          <tr>
            <th>ID</th>
            <th>Policy ID</th>
            <th>Prod Sheet Type</th>
            <th>Summary</th>
            <th>Preview</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          {filteredRows.length > 0 ? (
            filteredRows.map((row) => (
              <tr key={row.id}>
                <td>{row.id}</td>
                <td>{row.policyid || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.prod_sheet_type || <span className={styles.loader}>Pending</span>}</td>
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
                  {row.policyid && row.prod_sheet_type && row.summary ? (
                    <span>Completed</span>
                  ) : (
                    <button
                      className={styles.reloadButton}
                      onClick={() => handleReload(row.id)}
                      disabled={loadingRows.includes(row.id)}
                    >
                      {loadingRows.includes(row.id) ? (
                        <FontAwesomeIcon icon={faSpinner} spin className={styles.spinnerIcon} />
                      ) : (
                        <FontAwesomeIcon icon={faRotateRight} />
                      )}
                    </button>
                  )}
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="6" className={styles.noData}>
                No matching data found.
              </td>
            </tr>
          )}
        </tbody>
      </table>
    </div>
  );
};

export default MainContent;

// Mock API call for fetching data (Replace this with actual API logic)
const fetchData = async (id) => {
  // Simulate API call delay
  return new Promise((resolve) =>
    setTimeout(() => {
      resolve({
        policy_id: `Policy-${id}`,
        prod_sheet_type: "Type-A",
        summary: "Sample Summary",
        status: "Active",
        previewLink: "https://example.com/preview",
      });
    }, 1000)
  );
};




const handleReload = async (id) => {
  try {
    setLoadingRows((prev) => [...prev, id]); // Mark row as loading
    const singleclaimdata = await fetchData(id); // Fetch data for the specific ID
    console.log("Single Claim Data:", singleclaimdata);

    setRows((prevRows) =>
      prevRows.map((row) =>
        row.id === id
          ? {
              ...row,
              policyid: singleclaimdata.policy_id,
              prod_sheet_type: singleclaimdata.prod_sheet_type,
              summary: singleclaimdata.summary,
              status: singleclaimdata.status,
              previewLink: singleclaimdata.previewLink,
            }
          : row
      )
    );
  } catch (error) {
    console.error("Error reloading row data:", error);
  } finally {
    setLoadingRows((prev) => prev.filter((rowId) => rowId !== id)); // Remove row from loading state
  }
};
