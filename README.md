import React, { useEffect, useState } from 'react';
import axios from 'axios';
import AllDataTable from './AllDataTable'; // Assuming AllDataTable is in the same directory

const ManageClaims = () => {
  const [claimsData, setClaimsData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const fetchData = async () => {
    try {
      // Fetch data from the API
      const response = await axios.post("dummy1", { tasktype: "FETCH_ALL_ACT_CLAIMS" });

      // Log the raw data
      console.log("Received claims data:", response.data);

      // Ensure data is an array, otherwise convert it using Object.values
      const claimsArray = Array.isArray(response.data) ? response.data : Object.values(response.data);

      // Log the transformed data
      console.log("Claims data after conversion to array:", claimsArray);

      // Set the claims data to the state
      setClaimsData(claimsArray);
    } catch (err) {
      console.error("Error fetching data:", err);
      setError("Failed to fetch claims data.");
    } finally {
      setLoading(false);
    }
  };

  // Fetch data when the component mounts
  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div>
      {loading && <p>Loading...</p>}
      {error && <p>{error}</p>}
      {!loading && !error && <AllDataTable data={claimsData} />}
    </div>
  );
};

export default ManageClaims;



import React from 'react';

const AllDataTable = ({ data }) => {
  // Log the data to verify its structure
  console.log("Received data in AllDataTable:", data);

  // Ensure data is an array (in case it's an object, we convert it to array)
  const claimsArray = Array.isArray(data) ? data : Object.values(data);

  // Log the data after conversion
  console.log("Data after conversion to array:", claimsArray);

  if (claimsArray.length === 0) {
    return <p>No claims data available.</p>;
  }

  return (
    <table>
      <thead>
        <tr>
          <th>Claim ID</th>
          <th>Claim Type</th>
          <th>Summary</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        {claimsArray.map((claim, index) => (
          <tr key={index}>
            <td>{claim.claimid}</td>
            <td>{claim.claimtype}</td>
            <td>{claim.briefsummary}</td>
            <td>{claim.status}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
};

export default AllDataTable;
