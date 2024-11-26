<div className={styles.mainContent}>
  {/* Grid Container */}
  <div className={styles.gridContainer}>
    {/* Document Preview Section */}
    <div className={styles.previewSection}>
      <h3>Document Preview</h3>
      {data?.document_name?.length > 0 ? (
        <ul className={styles.documentList}>
          {data.document_name.map((doc, index) => (
            <li key={index}>
              <button
                className={styles.previewButton}
                onClick={() => openModal(doc.S)}
              >
                Preview {index + 1}
              </button>
            </li>
          ))}
        </ul>
      ) : (
        <p>No document available</p>
      )}
    </div>

    {/* Extract Content Section */}
    <div className={styles.extractContentSection}>
      <h3>Extract Content</h3>
      {loading ? (
        <p>Loading...</p>
      ) : data?.total_extracted_data ? (
        <pre className={styles.extractedData}>
          {JSON.stringify(JSON.parse(data.total_extracted_data), null, 2)}
        </pre>
      ) : (
        <p>No data available</p>
      )}
    </div>
  </div>

  {/* Verify Button Positioned Below */}
  <div className={styles.buttonContainer}>
    <button
      className={styles.verifyButton}
      onClick={() => console.log("Verification process started")}
    >
      Verify
    </button>
  </div>
</div>
