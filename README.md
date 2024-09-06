import Select from 'react-select';

const ClaimAssistComponent = () => {
  // Options for Claim Category and Claim IDs
  const claimCategoryOptions = [
    { value: 'Surrender Claim', label: 'Surrender Claim' },
    { value: 'Retirment Claim', label: 'Retirment Claim' },
    { value: 'Death Claim', label: 'Death Claim' }
  ];

  const claimIdOptions = claimIds.map(claimId => ({
    value: claimId,
    label: claimId
  }));

  return (
    <>
      {/* HEADER SECTION - BEGIN */}
      <div style={{ overflow: 'hidden', position: 'fixed', top: '0', width: '100%', zIndex: '1000' }}>
        <div style={{ overflow: 'hidden', backgroundColor: '#FFFFFF', padding: '-1px', color: '#0F5FDC', display: 'flex', justifyContent: 'space-between', alignItems: 'center', borderBottom: '1px solid #DBC5FF' }}>
          <div style={{ display: 'flex', overflow: 'hidden', color: '#0F5FDC', fontSize: '7px', margin: 'auto' }}>
            <h1>HCLTech</h1>
          </div>
        </div>
      </div>
      {/* HEADER SECTION - END */}

      <div style={{ marginTop: '3.5em', textAlign: 'center' }}>
        <h1 style={{ color: 'white', fontSize: '24px' }}>ClaimAssist</h1>
        <h3 style={{ color: 'white', fontSize: '19px' }}>Empower your claim process to minimize Claim Denial and maximize reimbursements</h3>
      </div>

      <div className="rounded-rectangle-entirepage" style={{ margin: '30px auto', background: '#FFFFFF' }}>
        <div style={{ display: 'flex' }}>
          <div className="rounded-rectangle" style={{ marginLeft: 5, width: fileNames.length > 0 ? '520px' : '830px' }}>
            <div style={{ fontFamily: 'verdana', marginTop: '1.5em', marginBottom: '1em', fontSize: '14px', alignSelf: 'center', padding: '3px', border: 'none', borderRadius: '5px', cursor: 'pointer', color: '#000000', display: 'flex' }}>
              <div>
                <div style={{ margin: 'auto', color: '#1E3979' }}>
                  <div style={{ display: 'flex', marginBottom: '1em' }}>
                    <div style={{ marginRight: '1em', marginTop: '0.2em', fontFamily: 'Verdana', color: 'black', fontWeight: 'bold' }}>Choose Claim Category</div>
                    
                    <Select
                      options={claimCategoryOptions}
                      value={claimCategoryOptions.find(option => option.value === claimCategoryvalue)}
                      onChange={(selectedOption) => handleOptionChangeForDropdownforclaimcategory(selectedOption.value)}
                      placeholder="--Select--"
                      styles={{ container: (provided) => ({ ...provided, width: 200 }) }}
                    />
                  </div>
                </div>

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

            {selectedOption === 'Existing' && (
              <>
                <div style={{ fontFamily: 'verdana', fontSize: '16px', marginTop: 10, alignSelf: 'center', padding: '3px', border: 'none', borderRadius: '5px', cursor: 'pointer', color: '#000000', display: 'flex' }}>
                  <div style={{ marginRight: '1em', marginTop: '0.2em' }}>Choose Claim Id</div>
                  <div>
                    <Select
                      options={claimIdOptions}
                      value={claimIdOptions.find(option => option.value === inputText)}
                      onChange={(selectedOption) => handleOptionChangeForDropdown(selectedOption.value)}
                      placeholder="--Select--"
                      styles={{ container: (provided) => ({ ...provided, width: 200 }) }}
                    />
                  </div>
                </div><br />
              </>
            )}
            
            {/* The rest of your code remains the same */}
            {/* Your existing upload functionality, preview, and footer */}

          </div>
        </div>
      </div>
    </>
  );
};

export default ClaimAssistComponent;
