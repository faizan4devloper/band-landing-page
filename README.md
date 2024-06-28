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














import React, { useState, useEffect} from 'react';
import './VerifierDashboard.css'; // Import the CSS file
import axios from 'axios';
 
   const VerifierDashboard = () => {
       const [data, setData] = useState([]);
    //   const [data, setData]= useState()
       useEffect(() => {
         fetchData();
       }, []);
    const fetchData = async () => {
         try {
           const response = await axios.get('https://3pey37cumu3bft2cqhcziwe3ea0jaqlp.lambda-url.us-east-1.on.aws/');
           console.log(response.data);
           setData(response.data);
         } catch (error) {
           console.error('Error fetching data:', error);
         }
       };
//   const response1 = await axios.post('https://qlssfc0k8l.execute-api.us-east-1.amazonaws.com/verify/iassureclaim', {
//         claimform_fileName: fileList1[0].name,
//         claimform_filebody: fileContent1,
//         payload: "claimform_upload"
//       });
    // const uploadToS3presignedurl = async (presignedUrl, file) => {
    //       const dashboarditems = await axios.get('https://3pey37cumu3bft2cqhcziwe3ea0jaqlp.lambda-url.us-east-1.on.aws/', {
    //         });
    // }
     // Dummy data for the table
     const claims = [
       {
         claimCategory: 'Health',
         claimId: '12345',
         policyId: '54321',
         submissionDate: '2023-04-15',
         status: 'Pending',
       },
       {
         claimCategory: 'Auto',
         claimId: '67890',
         policyId: '09876',
         submissionDate: '2023-04-10',
         status: 'Approved',
       },
       {
         claimCategory: 'Home',
         claimId: '13579',
         policyId: '24680',
         submissionDate: '2023-04-05',
         status: 'Rejected',
       },
       {
         claimCategory: 'Travel',
         claimId: '97531',
         policyId: '86420',
         submissionDate: '2023-04-01',
         status: 'Pending',
       },
       {
         claimCategory: 'Life',
         claimId: '24680',
         policyId: '13579',
         submissionDate: '2023-03-30',
         status: 'Approved',
       },
     ];
 
     return (
<div>
<div style={{ overflow: 'hidden', position: 'fixed',top: '0', width: '100%', zIndex: '1000' }}>
<div  style={{ overflow: 'hidden',backgroundColor: '#FFFFFF', padding: '-1px', color: '#0F5FDC', display: 'flex', justifyContent: 'space-between', alignItems: 'center',borderBottom: '1px solid #DBC5FF' }}>
<div  style={{ display: 'flex',overflow: 'hidden',color:'#0F5FDC',fontSize: '7px',margin: 'auto' }}>
<h1>HCLTech</h1>
</div>
</div>
</div>
<div className="claims-table-container">
<h1 className="heading">Verifiers Dashboard</h1>
<table className="claims-table">
<thead>
<tr>
<th>Claim Category</th>
<th>Claim Id</th>
<th>Policy Id</th>
<th>Submission Date</th>
<th>Status</th>
</tr>
</thead>
<tbody>
                 {claims.map((claim, index) => (
<tr key={index}>
<td>{claim.claimCategory}</td>
<td>{claim.claimId}</td>
<td>{claim.policyId}</td>
<td>{claim.submissionDate}</td>
<td>
<span
                         className={`status-label ${
                           claim.status.toLowerCase() === 'pending'
                             ? 'pending'
                             : claim.status.toLowerCase() === 'approved'
                             ? 'approved'
                             : 'rejected'
                         }`}
>
                         {claim.status}
</span>
</td>
</tr>
                 ))}
</tbody>
</table>
</div>
<div>
<h1>My Component</h1>
<ul>
                   {data.map((item, index) => (
<li key={index}>{item.name}</li>
                   ))}
</ul>
</div>
<div className="footer" style={{ marginTop:'1em',position:'sticky',bottom:0, left: 0, width: '100%', height: 20, background: 'white', borderTop: '1px #DBC5FF solid', display: 'flex', justifyContent: 'center', alignItems: 'center'}}>
<span style={{color: '#171719', fontSize: '10px', fontFamily: 'Montserrat', fontWeight: '500', letterSpacing: '0.28px', wordWrap: 'break-word'}}>© 2023 All Rights Reserved to </span>
<span style={{color: '#0F5FDC', fontSize: '10px', fontFamily: 'Montserrat', fontWeight: '500',letterSpacing: '0.28px', wordWrap: 'break-word'}}>HCLTech</span>
</div>
</div>
     );
   };
 
   export default VerifierDashboard;
has context menu











