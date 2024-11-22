import React, { useState, useEffect } from "react";
import axios from "axios";
import AllDataTable from "./AllDataTable";

const ManageClaims = () => {
  const [claimsData, setClaimsData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Fetch data from API
    const fetchData = async () => {
      try {
      const response = await axios.post("dummy1", {
        tasktype: "FETCH_ALL_ACT_CLAIMS",
      } catch (err) {
        setError("Failed to fetch claims data.");
      } finally {
        setLoading(false);
      }
    };
    
     
    fetchData();
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>{error}</p>;

  return (
    <div className="manage-claims">
      <h1>Manage Claims</h1>
      <AllDataTable data={claimsData} />
    </div>
  );
};

export default ManageClaims;
