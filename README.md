import React, { useState} from 'react';
import './VerifierDashboard.css'; // Import the CSS file
import axios from 'axios';
 
   const VerifierDashboard = () => {
       const [data, setData]= useState()
//   const response1 = await axios.post('https://qlssfc0k8l.execute-api.us-east-1.amazonaws.com/verify/iassureclaim', {
//         claimform_fileName: fileList1[0].name,
//         claimform_filebody: fileContent1,
//         payload: "claimform_upload"
//       });
    const uploadToS3presignedurl = async (presignedUrl, file) => {
          const dashboarditems = await axios.get('https://3pey37cumu3bft2cqhcziwe3ea0jaqlp.lambda-url.us-east-1.on.aws/', {
            });
    }






import React, { useState, useEffect } from 'react';
import axios from 'axios';
import './VerifierDashboard.css'; // Import the CSS file

const VerifierDashboard = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Fetch data from the API when the component mounts
  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.get('https://3pey37cumu3bft2cqhcziwe3ea0jaqlp.lambda-url.us-east-1.on.aws/');
        setData(response.data);
        setLoading(false);
      } catch (error) {
        setError(error);
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  // Render loading state, error state, and fetched data
  return (
    <div className="verifier-dashboard">
      <h1>Verifier Dashboard</h1>
      {loading && <p>Loading data...</p>}
      {error && <p>Error loading data: {error.message}</p>}
      {data && (
        <div className="dashboard-content">
          {/* Render your data here */}
          {data.map((item, index) => (
            <div key={index} className="dashboard-item">
              <p><strong>ID:</strong> {item.id}</p>
              <p><strong>Name:</strong> {item.name}</p>
              <p><strong>Status:</strong> {item.status}</p>
              {/* Add more fields as needed */}
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default VerifierDashboard;
