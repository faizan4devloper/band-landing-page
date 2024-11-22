import React, { useEffect, useState } from 'react';
import axios from 'axios';
import AllDataTable from './AllDataTable';

const ManageClaims = () => {
  const [claimsData, setClaimsData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const fetchData = async () => {
    try {
      const response = await axios.post("dummy", { tasktype: "FETCH_ALL_ACT_CLAIMS" });
      console.log("Raw API Response:", response.data);

      const claimsArray = Array.isArray(response.data)
        ? response.data
        : Object.values(response.data || {});
      console.log("Transformed data to array:", claimsArray);

      setClaimsData(claimsArray);
    } catch (err) {
      console.error("Error fetching data:", err);
      setError("Failed to fetch claims data.");
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div>
      {loading && <p>Loading...</p>}
      {error && <p>{error}</p>}
      {console.log("Loading:", loading, "Error:", error, "Claims Data:", claimsData)}
      {!loading && !error && <AllDataTable data={claimsData} />}
    </div>
  );
};

export default ManageClaims;

import React from 'react';

const AllDataTable = ({ data }) => {
  console.log("Props received in AllDataTable:", data);

  const claimsArray = Array.isArray(data) ? data : Object.values(data || {});
  console.log("Data converted to array in AllDataTable:", claimsArray);

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
