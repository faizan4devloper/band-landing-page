/*global localStorage*/
import React, { useEffect, useState, useRef } from 'react';
import  { createContext, useContext } from 'react';
import './index.css';
import axios from 'axios';
import { FaTimes, FaCheck } from 'react-icons/fa';
import { Link, useNavigate } from 'react-router-dom';
import Documentdetails from './components/DocumentDetails';
import  { useChatbotState } from './SharedStateContext';
import  { useSummaryState } from './SharedStateContext';
import S3FilePreview from './components/S3FileViewer';
import { S3 } from 'aws-sdk';
import { useSharedState } from './SharedStateContext';
import AWS from 'aws-sdk';
import { v4 as uuidv4 } from 'uuid';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faTrash } from '@fortawesome/free-solid-svg-icons';

const Landing = () => {
  
  const { sharedValue, setSharedValue } = useSharedState();
  const [file1, setFile1] = useState(null);
  const [file2, setFile2] = useState(null);
  const [responseData, setResponseData] = useState(null);
  const [buttonClicked, setButtonClicked] = useState(false);
  const [showPopup, setShowPopup] = useState(false);
  const [showServerErrorPopup, setShowServerErrorPopup] = useState(false);
  const [inputValue, setInputValue] = useState('');
  const [fileNames, setFileNames] = useState([]);
  const [clickedDocument, setClickedDocument] = useState(null);
  const [verificationCompleted, setVerificationCompleted] = useState(false);
  const [noFilesUploaded, setNoFilesUploaded] = useState(false);
  const [loading, setLoading] = useState(false);
  const [fileContent, setFileContent] = useState(null);
  const [sharedVariable, setSharedVariable] = useState('');
  const [claimid,setClaimid] = useState('');
  const [servererror, setServererror] = useState(false);
  const [claimIds, setClaimIds] = useState(null);
  const { resetChatbotState } = useChatbotState();
  const { resetSummaryState } = useSummaryState();
  const [claimIdGenerated, setClaimIdGenerated] = useState('');
  const [newClaimId, setNewClaimId] = useState('');
  const [inputText, setInputText] = useState('');
  const [claimCategoryvalue, setClaimCategoryvalue] = useState('');
  const [uploading, setUploading] = useState(false);
  const [fetchedfilesfroms3, setFetchedfilesfroms3] = useState(false);
  const [s3ForFileUpload, setS3ForFileUpload] = useState(null);
  // state variables for checking file existence in s3
  const [filePath, setFilePath] = useState('');
  const [isPrimaryPathChecked, setIsPrimaryPathChecked] = useState(false);
  // For radio buttons
  const [selectedOption, setSelectedOption] = useState('');
  const [doctype, setDoctype] = useState('');
  const [docstatus, setDocstatus]  = useState('');
  // For Document category - BEGIN
  const [categoryValues, setCategoryValues] = useState({});
  const handleOptionChangeForDropdown = (event) => {
      setSharedValue('filelist','');
      setInputText(event.target.value);
      setNewClaimId(event.target.value);
      console.log("LOOPIN",newClaimId);
    };
  
  const handleOptionChangeForDropdownforclaimcategory = (event) => {
      setClaimCategoryvalue(event.target.value);
       setSharedValue('claimcategory', event.target.value);
    };
 
  const handleOptionChange = (event) => {
    console.log("IPTEST1",inputText);
    setSelectedOption(event.target.value);
    setFileNames([]);
    setClickedDocument(null);
    if(event.target.value === "New"){
        setFetchedfilesfroms3(true);
    }
    
    if(event.target.value === "New" &&  !claimIdGenerated){
    console.log("ENTEREDIFCONDITION");
          
          setClaimIdGenerated(true);
          setNewClaimId(generateClaimId());
   }
          

  };

  const [messageIndex, setMessageIndex] = useState(0);
  const navigate = useNavigate();
  const redirectToMainPage = () => {
    // Redirect to the main page
    navigate('/');
  };
  
  let timeout = 2 * 60 * 1000;

  const handleDocumentClick = (document) => {
    setClickedDocument((prevDocument) => (prevDocument === document ? null : document));
    // setFileContent(null); // Reset fileContent state
    setFilePath(`claimassist/claimforms/${newClaimId}/${clickedDocument}`);
    setShowPopup(true);
  };
  
  const handleButtonClick = () => {
    // Perform any action you want with the input value
    console.log(inputValue);
  };

  function generateClaimId() {
    const timestamp = new Date().getTime(); // Get current timestamp
    console.log("TIMESTAMPTEST", timestamp);
    const maxRandomNumber = 999999999; // Maximum 9-digit number
    const randomNumber = Math.floor(Math.random() * maxRandomNumber); // Generate a random number less than the maximum
    const claimId = "CL" + timestamp + randomNumber.toString().padStart(9, '0'); // Combine timestamp and random number
    return claimId.slice(0, 10); // Take the first 12 characters to ensure max 10 digits
  }

 const handleFileChange1 = async (event) => {
   if(!newClaimId){
     alert('Please enter claimid');
      return;
    }
   else{
    const selectedFiles = Array.from(event.target.files);
    // Check if more than one file is selected
   if (selectedFiles.length > 1) {
     alert('Please select only one file');
     return;
   }
    const file = selectedFiles[0]; // Since only one file is allowed
   
   // Reset file state to handle file overwrite
   setFile1([file]);
   setFileNames([file.name]); // Overwrite file names
   setSharedValue('filelist', [file.name]);
    // Upload to S3 immediately after selecting the files
    setUploading(true);
    
    // Loop through selected files
    for (const file of selectedFiles) {
        // Get file extension
        const fileExtension = file.name.split('.').pop();

        // Send Axios request to Lambda
        try {
            const response = await axios.get('https://109bsrkzi1.execute-api.us-east-1.amazonaws.com/dev/presignedurl', {
                params: {
                    extension: fileExtension,
                    filename: file.name,
                    claimid: newClaimId
                }
            });
            console.log("REPSONSE SIGNED ", response);
            const { presignedUrl, key } = response.data;
            // Upload file to S3 using presigned URL
            await uploadToS3presignedurl(presignedUrl, file);
            const sleep = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
   
            await sleep(60000); 
            const doc_category = await  axios.get('https://109bsrkzi1.execute-api.us-east-1.amazonaws.com/dev/claimassistcategory', {
                                      params: {
                                          claimid: newClaimId,
                                      },
                                  });
            const attr_haiku_str = doc_category.data.extracted_data;
            const attr_haiku = JSON.parse(attr_haiku_str);
            console.log("EXTRACTED DOC CONTENTS OF ATTR HAIKU",attr_haiku);
            // console.log("POLICY NUMBER",attr_haiku.PAYMENT_INSTRUCTION_FORM['POLICY_NUMBER']);
            setResponseData(attr_haiku);
            // setDoctype("PIF");
            
            setSharedValue('doccategory',doc_category.data.category);
            setSharedValue("unfilledpercent",doc_category.data.unfilledpercent);
            setSharedValue('docdetails',attr_haiku);
            console.log("ATTR HAIKU LATEST TYPE 1",typeof attr_haiku);
            console.log("ATTR HAIKU LATEST",attr_haiku.PAYMENT_INSTRUCTION_FORM);
            console.log("ATTR HAIKU LATEST TYPE",typeof attr_haiku.PAYMENT_INSTRUCTION_FORM);
            setSharedValue('policynum',attr_haiku.PAYMENT_INSTRUCTION_FORM.POLICY_NUMBER);
            setDoctype(doc_category.data.category);

            setUploading(false);
            } catch (error) {
              setUploading(false);
                console.error('Error:', error);
            }
        }
    }
 };
  
  
  const uploadToS3presignedurl = async (presignedUrl, file) => {
        console.log("RESPONSE SIGNED INSIDE FUNCTION", presignedUrl);
        console.log("FILE NAME INSIDE FUN",file);
        console.log("FILE TYPE",file.type);
        // console.log("CLAIM ID FOR DOC CATEGORY",newClaimId);
        const fileExtension = file.name.split('.').pop();
        const contentType = `application/${fileExtension}`;
        const absolute_file_path = `claimassist/claimforms/${newClaimId}/${file.name}`;
        console.log("FILE NAME FOR DOC CATEGORY",absolute_file_path);
        console.log("CONTENTTYPE",contentType);
        try {
            await axios.put(presignedUrl, file, {
                headers: {
                    'Content-Type': contentType,
                },
            });
            
            // call document extract lambda
            const doc_extract = await axios.get('https://109bsrkzi1.execute-api.us-east-1.amazonaws.com/dev/doc-extract', {
              params: {
        filename: absolute_file_path,
        claimid: newClaimId,
              }
    });
  // Sleep for 2 seconds
    // const doc_category = await  axios.get('https://109bsrkzi1.execute-api.us-east-1.amazonaws.com/dev/claimassistcategory', {
    //         params: {
    //             claimid: newClaimId,
    //         },
    //     });
    
    console.log("EXTRACTED DOC CONTENTS",doc_extract);
    console.log("EXTRACTED DOC CONTENTS",doc_extract.data);
    // const rawtext = doc_category.data.extracted_data.data.rawtext;
  
    
    
        } catch (error) {
            console.error('Error uploading file:', error);
        }
    };
  
  const fetchFileNamesFromS3 = async (claimid, fileNames) => {
    
    try {
       if (!claimid) {
        console.log("New Claim ID is empty.");
        return []; // Return an empty array
      }
      const response = await axios.get('https://109bsrkzi1.execute-api.us-east-1.amazonaws.com/dev/presignedurl', {
        params: {
          payload: 'fetch',
          subfolder: `claimassist/claimforms/${newClaimId}`
        }
      });
       const doc_category = await axios.get('https://109bsrkzi1.execute-api.us-east-1.amazonaws.com/dev/claimassistcategory', {
              params: {
        claimid: newClaimId,
       
              }
    });
    
    const attr_haiku_str = doc_category.data.extracted_data;
    const attr_haiku = JSON.parse(attr_haiku_str);
    console.log("EXTRACTED DOC CONTENTS OF ATTR HAIKU",attr_haiku);
    // console.log("POLICY NUMBER",attr_haiku.PAYMENT_INSTRUCTION_FORM['POLICY_NUMBER']);
    setResponseData(attr_haiku);
    // setDoctype("PIF");
    
    setSharedValue('doccategory',doc_category.data.category);
    setSharedValue("unfilledpercent",doc_category.data.unfilledpercent);
    setSharedValue('docdetails',attr_haiku);
    console.log("ATTR HAIKU LATEST TYPE 1",typeof attr_haiku);
    console.log("ATTR HAIKU LATEST",attr_haiku.PAYMENT_INSTRUCTION_FORM);
    console.log("ATTR HAIKU LATEST TYPE",typeof attr_haiku.PAYMENT_INSTRUCTION_FORM);
    setSharedValue('policynum',attr_haiku.PAYMENT_INSTRUCTION_FORM.POLICY_NUMBER);
    setDoctype(doc_category.data.category);

    
      console.log("RESPONSE SIGNED ", response);
      console.log("FILES",response.data.fileslist);
      const filelist = response.data.fileslist;
      setFetchedfilesfroms3(true);
      const allowedExtensions = /\.(png|jpg|pdf|jpeg)$/i;
      const filteredFiles = filelist.filter(file => allowedExtensions.test(file));
      return filteredFiles;
    } catch (error) {
      console.error('Error:', error);
      setFetchedfilesfroms3(true);
      return [];
    }
  };

useEffect(() => {
  if (selectedOption === "Existing") {
    setFetchedfilesfroms3('fetching');
    console.log("DROPDOWNSELECTIONNNNNN",inputText);
    
      fetchFileNamesFromS3(inputText)
      .then(retrievedFileNames => {
        // setFileNames(prevFileNames => [...prevFileNames, ...retrievedFileNames]);
        setFileNames(prevFileNames => [...retrievedFileNames]);
        
      })
      .catch(error => {
        console.error('Error:', error);
      });
  }
    
}, [selectedOption,inputText]); // Empty dependency array ensures this effect runs only once
console.log("FINALLISTOFFILENAMES",fileNames);
console.log("FINALLISTOFFILENAMESTYPE",typeof fileNames);

 

  const readFileAsBase64 = (file) => {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = () => resolve(reader.result.split(',')[1]);
      reader.onerror = (error) => reject(error);
      reader.readAsDataURL(file);
    });
  };
   
// File upload to Lambda - begin
const fileInputRef1 = useRef(null);  // Change the ref name for the first file
const fileInputRef2 = useRef(null);


console.log("SELECTTTTTEDDD",selectedOption);

console.log("CLAIMIDDISPLAY",claimid);
const messages = ["Claim processing has started",`Claim with ${newClaimId} processing In Progress`]

console.log("SOOOO",selectedOption);

useEffect(() => {
    const fetchClaimIds = async () => {
      try {
        const response = await axios.get('https://109bsrkzi1.execute-api.us-east-1.amazonaws.com/dev/claimassistexistingclaimidlist');
        setClaimIds(response.data); // Update claimIds state with fetched data
       
        
      } catch (error) {
        console.error('Error fetching claim IDs:', error);
        setClaimIds(null); // or handle the error in some way
      }
    };

    if (selectedOption === 'Existing') {
      fetchClaimIds(); // Call fetchClaimIds function if selectedOption is 'Existing'
    }
  }, [selectedOption]); // Run this effect whenever selectedOption changes

console.log("CLAIM IDS EXISTING",claimIds);

const DocumentStatus = ({ doctype, onDelete }) => {
    const validDoctypes = [
        "Payment Instruction Form",
        "Lost Policy Form",
        "Original Policy Document"
    ];

    const isValid = validDoctypes.includes(doctype);
    setDocstatus(isValid);
    return (
        <div style={{ marginLeft: '0em', width: '100%' }}>
            <span>{doctype}&nbsp;&nbsp;</span>
            <span style={{ color: isValid ? 'green' : 'red' }}>
                {isValid ? <span style={{ color: 'green' }}>&#10004;</span> : <span style={{ color: 'red' }}>&#10060;</span>}
            </span>
            {!isValid && (
                <FontAwesomeIcon
                    icon={faTrash}
                    style={{ cursor: 'pointer', marginLeft: '10px' }}
                    onClick={onDelete}
                />
            )}
        </div>
    );
};
 const handleDelete = () => {
        console.log('File deleted');
        setClaimCategoryvalue('');
        setSelectedOption('');
         setFileNames([]);
         setDocstatus(true);
         setSelectedOption('');
    };



const handleFileUpload = async () => {
  resetChatbotState(); 
  resetSummaryState();
  if (!claimCategoryvalue){
     alert('Please select claim category');
      return;
  }
   if (!selectedOption){
    alert('Please select claim type');
      return;
  }
  if (selectedOption === "Existing"){
  if ((!inputText) || (inputText === "--Select--")) {
      alert('Please enter claimid');
      return;
    }}
  console.log("DOC STATUS",docstatus);
  if (docstatus === false){
    alert('Uploaded Document is invalid for further processing');
    // setFile1(null);
      return;
  }  
  const fileInput1 = fileInputRef1.current;
  // const fileInput2 = fileInputRef2.current;
  console.log("FILE INPUT oNE",fileInput1);
  console.log("FILE INPUT ONE LENGTH",fileInput1.files.length);
  if ((fileInput1.files.length === 0) && (selectedOption === 'New')) {
   console.log("NO FILES UPLOADED");
   alert('No files uploaded');
   return;
    // setNoFilesUploaded(true);
    // setShowPopup(true); // Show the popup
  } 
  let fileList1;
  if (fileNames){
    setSharedValue('uploadedfilename',fileNames[0]);
  }
  if ((fileInput1 && fileInput1.files.length > 0) ) {
        fileList1 = fileInput1.files;
        console.log("DUMMY FILE NAME",fileList1[0].name);
        
            }
   
  if(fileList1){
    
              setSharedValue('uploadedfilename', fileList1[0].name);}
              setSharedValue('claimid',newClaimId);
              navigate('./components/DocumentDetails',{ state: { data: responseData } });
};
console.log("SHARED",sharedValue);

console.log("responsedatafromlambda",responseData);
  useEffect(() => {
    const checkFileExistence = async () => {
      const primaryPath = `claimassist/claimforms/${newClaimId}/${clickedDocument}`;
      const secondaryPath = `claimassist/claimforms/${newClaimId}/additionaldocuments/${clickedDocument}`; // Adjust this path as needed
      
     
      setFilePath(primaryPath);
      setIsPrimaryPathChecked(true);
    };

    if (clickedDocument) {
      checkFileExistence();
    }
  }, [clickedDocument, newClaimId]);
   
// Function to check if file exists in s3 path  
const isFileExists = async (filePath) => {
  try {
//     const response = await axios.head(`https://aimlusecasesv1.s3.amazonaws.com/${filePath}`);
    const response = await axios.get(`https://aimlusecasesv1.s3.amazonaws.com/${filePath}`);
    return response.status === 200;
  } catch (error) {
    console.error('Error checking file existence:', error);
    return false;
  }
};  
console.log("FILENAMES TYPE",typeof fileNames); 
console.log("FILENAMES", fileNames); 
// JSX - START    
  return (
<>
{/* HEADER SECTION - BEGIN */}
<div style={{ overflow: 'hidden', position: 'fixed',top: '0', width: '100%', zIndex: '1000' }}>
  <div  style={{ overflow: 'hidden',backgroundColor: '#FFFFFF', padding: '-1px', color: '#0F5FDC', display: 'flex', justifyContent: 'space-between', alignItems: 'center',borderBottom: '1px solid #DBC5FF' }}>
    <div  style={{ display: 'flex',overflow: 'hidden',color:'#0F5FDC',fontSize: '7px',margin: 'auto' }}>
      <h1>HCLTech</h1>
    </div>
  </div>
</div>
{/* HEADER SECTION - END */}

<div style={{marginTop:'3.5em',textAlign:'center'}}>
<h1 style={{color:'#000000',fontSize:'24px',color:'white'}}>ClaimAssist</h1>

<h3 style={{color:'#000000',fontSize:'19px',color:'white'}}>Empowerrr your claim process to minimize Claim Denial and maximize reimbursements</h3></div>
<div className="rounded-rectangle-entirepage" style={{ margin: '30px auto',background:'#FFFFFF' }}>
  <div style={{display:'flex'}}>
  <div className="rounded-rectangle" style={{ marginLeft: 5,width: fileNames.length > 0 ? '520px' : '830px'}}>
  <div style={{ display: 'flex', justifyContent: 'space-between', width: '100%' }}>
  </div>
 
 <div style={{ fontFamily: 'verdana', marginTop:'1.5em',marginBottom:'1em',fontSize: '14px',  alignSelf: 'center', padding: '3px', border: 'none', borderRadius: '5px', cursor: 'pointer', color: '#000000', display: 'flex' }}>
<div>
 
      <div style={{ margin: 'auto',color:'#1E3979' }}>
        <div style={{display:'flex',marginBottom:'1em'}}>
         <div style={{ marginRight: '1em',marginTop:'0.2em',fontFamily:'Verdana',color: 'black',fontWeight:'bold' }}>Choose Claim Category</div>
 <select value={claimCategoryvalue} onChange={handleOptionChangeForDropdownforclaimcategory} style = {{height:'30px'}}>
  <option value="--Select--">--Select--</option>
    <option key={"Surrender Claim"} value={"Surrender Claim"}>{"Surrender Claim"}</option>
    <option key={"Retirment Claim"} value={"Retirment Claim"}>{"Retirment Claim"}</option>
    <option key={"Death Claim"} value={"Death Claim"}>{"Death Claim"}</option>
    
</select>  
</div></div>
<b>Select type of claim: </b>
      <label>
        <input
          type="radio"
          value="New"
          checked={selectedOption === 'New'}
          onChange={handleOptionChange}
        />
        New
      </label>&nbsp;
      <label>
        <input
          type="radio"
          value="Existing"
          checked={selectedOption === 'Existing'}
          onChange={handleOptionChange}
        />
        Existing
      </label>
    </div>
  </div>
  {console.log("SELECTEDOPTION",selectedOption)}
  {selectedOption === 'Existing' && (
  <>
  <div style={{fontFamily:'verdana',fontSize:'16px',marginTop: 10,alignSelf: 'center',padding: '3px',border: 'none',borderRadius: '5px',cursor: 'pointer',color:'#000000',display:'flex',}}>
      <div style={{ marginRight: '1em',marginTop:'0.2em' }}>Choose Claim Id</div>
<div>
     
      <select value={inputText} onChange={handleOptionChangeForDropdown} style = {{height:'30px'}}>
  <option value="--Select--">--Select--</option>
  {claimIds && claimIds.length > 0 ? (
        claimIds.map(claimId => (
          <option key={claimId} value={claimId}>{claimId}</option>
        ))
      ) : (
        <option value="" disabled>Retrieving existing claims</option>
      )}
</select>
            </div>

    </div><br/> </>
    )}
 <div style={{ flex: 1, display: 'flex', alignItems: 'center',margin:'auto' }}>
      <div>
        <b>Upload Claim Form</b>
      </div>
      <div >
           {selectedOption && newClaimId && (
        <label htmlFor="treeFile1" style={{ cursor: 'pointer' }}>
          <span role="img" aria-label="tree" >
          <img src={process.env.PUBLIC_URL + '/uploadicon.png'} style={{height:'30px',width:'40px'}}></img>
          </span>
          
          <input type="file" id="treeFile1" onChange={handleFileChange1} style={{ display: 'none' }} ref={fileInputRef1} multiple/>
      
        </label>
)}
      </div>
    </div>
  {console.log("FETCHEDSTATUSFROMS3",fetchedfilesfroms3)}
  <button
  onClick={handleFileUpload}
  style={{
    fontSize: '16px',
    width: '150px',
    marginTop: 1,
    alignSelf: 'center',
    marginTop: '1em',
    padding: '3px',
    backgroundColor: docstatus === false||uploading || fetchedfilesfroms3 === 'fetching'? '#ccc' : '#0F5FDC', // Grey out when uploading
    color: docstatus === false||uploading || fetchedfilesfroms3 === 'fetching'? '#888' : '#fff', // Grey out the text when uploading
    border: 'none',
    borderRadius: '5px',
    cursor: docstatus === false||uploading || fetchedfilesfroms3 === 'fetching'? 'not-allowed' : 'pointer', // Change cursor when uploading
  }}
  disabled={docstatus === false ||uploading || fetchedfilesfroms3 === 'fetching'} // Disable the button when uploading
>
  {loading && (
    <div className="spinner-overlay" >
      <div className="spinner-container">
        <div className="spinner-text">{messages[messageIndex]}</div> &nbsp;&nbsp;
        <div className="spinner"></div>
      </div>
    </div>
  )}
  Process
</button>

  </div>
  
  {fileNames.length > 0 && (
  
<div style={{ marginLeft: 10,marginRight: 10, border: '1px solid #000', borderRadius: '15px', width: '870px',  border: '1px solid blue', borderRadius: '15px' }}>
  <div style={{ marginLeft: 'auto', marginRight: 'auto', textAlign: 'center', marginTop: '0.5em' }}>
    <b>Uploaded Docs:</b>
  </div><br/>
  <div style={{ marginLeft: 0 }}>
   <table style={{ width: '100%', borderCollapse: 'collapse', marginTop: '0.3em' }}>
      <thead>
        <tr>
          <th style={{ textAlign: 'left', padding: '8px', fontWeight: 'bold' }}>Document Name</th>
          <th style={{ textAlign: 'left', padding: '8px', fontWeight: 'bold' }}>Document Type</th>
        </tr>
      </thead>
      <tbody>
        {[...new Set(fileNames)].map((fileName, index) => (
          <tr key={index}>
            <td style={{ padding: '8px' }}>
              {!uploading && (
                <li style={{ listStyleType: 'none', textAlign: 'left', color: clickedDocument === fileName ? 'green' : 'blue', fontWeight: 'bold' }}>
                  <Link onClick={() => handleDocumentClick(fileName)} style={{ textDecoration: 'underline', color: clickedDocument === fileName ? 'green' : 'blue' }}>
                    {fileName}
                  </Link>
                </li>
              )}
              {uploading && (
                <li style={{ listStyleType: 'none', textAlign: 'left' }}>
                  Uploading & Extracting {fileName}...
                </li>
              )}
            </td>
            <td style={{ padding: '8px' }}>
              {!uploading && (
                <DocumentStatus doctype={doctype} onDelete={handleDelete} />
              )}
            </td>
          </tr>
        ))}
      </tbody>
    </table>
  </div>
</div>)}
</div>
{
  clickedDocument ? (
    filePath ? (
      <div style={{ flex: 1, width: '830px', marginLeft: '0.5em', borderColor: 'blue', border: '1px solid blue', borderRadius: '15px', padding: '10px', marginTop: '1em' }}>
        <div>
          <p style={{ color: '#1E3979', marginTop: '1em', textAlign: 'center' }}>
            <b>Preview of {clickedDocument}</b>
          </p>
          <div style={{ width: '530px', border: '1px solid', padding: '1px', marginTop: '1em', justifyContent: 'center', margin: 'auto', overflow: 'auto' }}>
            <S3FilePreview key={filePath} fileName={filePath} style={{ width: '100%' }} />
          </div>
        </div>
      </div>
    ) : (
      <div style={{ flex: 1, width: '830px', marginLeft: '0.5em', borderColor: 'blue', border: '1px solid blue', borderRadius: '15px', padding: '10px', marginTop: '1em' }}>
        <div>
          <p style={{ color: '#1E3979', marginTop: '1em', textAlign: 'center' }}>
            <b>Preview of {clickedDocument}</b>
          </p>
          <div style={{ width: '530px', border: '1px solid', padding: '1px', marginTop: '1em', justifyContent: 'center', margin: 'auto', overflow: 'auto' }}>
            Fetching Preview
          </div>
        </div>
      </div>
    )
  ) : null
}



{console.log("STATE1",selectedOption)}
{noFilesUploaded && (selectedOption === 'New' && (
  <div className="popup-overlay">
    <div className="popup-content" style={{background:'#C0C0C0'}}>
      <span style={{color:'black',fontSize:'22px'}}><b>Alert! </b><br/><br/></span>
      <span style={{color:'black',fontFamily:'verdana',fontSize:'20px'}}>No files uploaded! Please select files before verifying.<br/><br/></span>
      <button style={{ width: '80px', height: '30px', padding: '8px 12px', borderRadius:'10px', border: '#0F5FDC', background:'#0F5FDC', cursor: 'pointer', color:'white', fontSize:'13px', margin:'auto' }} onClick={() => {
        setNoFilesUploaded(false);
        setShowPopup(false);
      }}>OK</button>
    </div>
  </div>
))}
 
{servererror && (
  <div className="popup-overlay">
    <div className="popup-content" style={{background:'#C0C0C0'}}>
      <span style={{color:'black',fontSize:'22px'}}><b>Alert! </b><br/><br/></span>
      <span style={{color:'black',fontFamily:'verdana',fontSize:'20px'}}>Backend server couldn't process your request,please try again later<br/><br/></span>
      <button style={{ width: '80px', height: '30px', padding: '8px 12px', borderRadius:'10px', border: '#0F5FDC', background:'#0F5FDC', cursor: 'pointer', color:'white', fontSize:'13px', margin:'auto' }} onClick={() => {
        setServererror(false);
        setShowServerErrorPopup(false);
      }}>OK</button>
    </div>
  </div>
)}
       </div>
       
       <div className="footer" style={{ position: 'relative', width: '100%', marginTop: clickedDocument ? '30px' : '17em', background: 'white', display: 'flex', justifyContent: 'center', alignItems: 'center'}}>
  <hr style={{  position:'absolute',marginTop:'-2em',left: 0, width: '100%', border: '1px #DBC5FF solid' }} />
  <span style={{color: '#171719', fontSize: '10px', fontFamily: 'Montserrat', fontWeight: '500', letterSpacing: '0.28px', wordWrap: 'break-word'}}>© 2023 All Rights Reserved to </span>
  <span style={{color: '#0F5FDC', fontSize: '10px', fontFamily: 'Montserrat', fontWeight: '500',letterSpacing: '0.28px', wordWrap: 'break-word'}}>HCLTech</span>
</div>

</>
  );
};

export default Landing;
