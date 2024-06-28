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
