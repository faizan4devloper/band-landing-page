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
