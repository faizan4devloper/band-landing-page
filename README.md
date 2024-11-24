
// import React, { useEffect, useState } from "react";
// import axios from "axios";
// import styles from "./DataTable.module.css";

// const DataTable = () => {
//   const [rows, setRows] = useState([]);
//   const [loading, setLoading] = useState(false);
//   const [error, setError] = useState("");

//   // Fetch data from the API
//   const fetchData = async () => {
//     setLoading(true);
//     setError(""); // Clear any previous errors
//     try {
//       const payload = {
//         tasktype: "FETCH_ALL_CLAIMS",
//       };

//       const headers = {
//         "Content-Type": "application/json",
//       };

//       const response = await axios.post("https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest", payload, {
//         headers,
//       });

//       console.log("API Response:", response.data);

//       // Adjusting the data mapping based on the console data structure
//       setRows(response.data || []); // Ensure it's mapped properly from response
//     } catch (err) {
//       setError("Failed to fetch data. Please try again.");
//       console.error("API Error:", err);
//     } finally {
//       setLoading(false);
//     }
//   };

//   // Fetch data on component mount
//   useEffect(() => {
//     fetchData();
//   }, []);

//   const handleReload = async (recNum) => {
//     try {
//       const payload = {
//         tasktype: "FETCH_SINGLE_CLAIM",
//         claimid: recNum,
//       };

//       const headers = {
//         "Content-Type": "application/json",
//       };

//       const response = await axios.post("dummy2", payload, {
//         headers,
//       });

//       console.log(`Reloaded Data for ${recNum}:`, response.data);

//       // Update specific row based on recNum
//       const updatedRow = response.data;
//       setRows((prevRows) =>
//         prevRows.map((row) =>
//           row.rec_number === recNum ? { ...row, ...updatedRow } : row
//         )
//       );
//     } catch (error) {
//       console.error("Error reloading row:", error);
//     }
//   };

//   return (
//     <div className={styles.tableContainer}>
//       {loading ? (
//         <p>Loading data...</p>
//       ) : error ? (
//         <p className={styles.error}>{error}</p>
//       ) : rows.length > 0 ? (
//         <table className={styles.table}>
//           <thead>
//             <tr>
//               <th>RecNum</th>
//               <th>Policy ID</th>
//               <th>Type</th>
//               <th>Summary</th>
//               <th>File Name</th>
//               <th>Actions</th>
//             </tr>
//           </thead>
//           <tbody>
//             {rows.map((row, index) => (
//               <tr key={index}>
//                 <td>{row.rec_number}</td>
//                 <td>{row.policy_id}</td>
//                 <td>{row.prod_sheet_type}</td>
//                 <td>{row.summary}</td>
//                 <td>{row.file_name}</td>
//                 <td>
//                   <button
//                     className={styles.reloadButton}
//                     onClick={() => handleReload(row.rec_number)}
//                   >
//                     Reload
//                   </button>
//                 </td>
//               </tr>
//             ))}
//           </tbody>
//         </table>
//       ) : (
//         <p className={styles.noData}>No data available</p>
//       )}
//     </div>
//   );
// };

// export default DataTable;





import React, { useEffect, useState } from 'react';
import axios from 'axios';
import styles from './DataTable.module.css';

const DataTable = () => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");

  const fetchData = async () => {
    setLoading(true);
    setError("");
    try {
      const payload = {
        tasktype: "FETCH_ALL_CLAIMS",
      };

      const headers = {
        "Content-Type": "application/json",
      };

      const response = await axios.post("dummy", payload, { headers });

      console.log("API Response:", response.data);

      // Extract values from the object
      const claimData = Object.values(response.data.allclaimdata || {});
      console.log("Extracted Claim Data:", claimData);

      setRows(claimData);
    } catch (err) {
      setError("Failed to fetch data. Please try again.");
      console.error("API Error:", err);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div className={styles.tableContainer}>
      {loading ? (
        <p>Loading data...</p>
      ) : error ? (
        <p className={styles.error}>{error}</p>
      ) : rows.length > 0 ? (
        <table className={styles.table}>
          <thead>
            <tr>
              <th>FileName</th>
              <th>RecNum</th>
              <th>Policy ID</th>
              <th>Type</th>
              <th>Summary</th>
              <th>Status</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {rows.map((row, index) => (
              <tr key={index}>
                <td>{row.file_name}</td>
                <td>{row.rec_number}</td>
                <td>{row.policy_id}</td>
                <td>{row.prod_sheet_type}</td>
                <td>{row.summary}</td>
                <td>{row.status || "Pending"}</td>
                <td>
                  <button className={styles.reloadButton}>Reload</button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      ) : (
        <p className={styles.noData}>No data available</p>
      )}
    </div>
  );
};

export default DataTable;
