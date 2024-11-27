const [previewUrl, setPreviewUrl] = useState(null); // State for static preview URL

const handleUpload = async () => {
  if (!file) {
    setMessage("Please select a file to upload.");
    return;
  }
  setUploading(true);
  try {
    const sanitizedFileName = file.name.replace(/\s+/g, "_");
    const response = await axios.post(
      "dummy",
      {
        payload: { filename: sanitizedFileName, filetype: "CL" },
      }
    );

    const { presignedUrl, key, recNum } = response.data;

    await axios.put(presignedUrl, file, {
      headers: { "Content-Type": file.type },
    });

    setRows((prevRows) => [
      ...prevRows,
      {
        recNum,
        policyid: "",
        type: "",
        summary: "",
        previewLink: presignedUrl,
        status: "Pending",
      },
    ]);

    setPreviewUrl(presignedUrl); // Set the uploaded document URL for preview
    setIsUploaded(true);
  } catch (error) {
    setMessage("Upload failed: " + error.message);
  } finally {
    setUploading(false);
  }
};

return (
  <div className={styles.container}>
    <div className={styles.sidebar}>
      <Sidebar
        onFileChange={handleFileChange}
        onUpload={handleUpload}
        uploading={uploading}
      />
    </div>
    <div className={styles.mainContent}>
      {isUploaded ? (
        <MainContent
          message={message}
          rows={rows}
          setRows={setRows}
          previewUrl={previewUrl} // Pass preview URL as prop
        />
      ) : (
        <p className={styles.infoMessage}>
          Please upload a document to view the data.
        </p>
      )}
    </div>
  </div>
);










const MainContent = ({ message, rows, setRows, previewUrl }) => {
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState(null);

  const handleReload = async (recNum) => {
    setLoading(true);
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_ACT_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post(`DUMMY`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      setData(response.data.allclaimactdata);
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoading(false);
    }
  };

  const renderReadableContent = (data) => {
    if (!data) return <p>No data available</p>;
    const parsedData = JSON.parse(data);

    return (
      <div className={styles.readableContent}>
        <h4>Claim Form Details</h4>
        <p><strong>Type:</strong> {parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE}</p>
        {/* More fields here */}
      </div>
    );
  };

  return (
    <div className={styles.mainContent}>
      {/* Left: Document Preview */}
      <div className={styles.previewSection}>
        <h3>Document Preview</h3>
        {previewUrl ? (
          <iframe
            src={previewUrl}
            title="Document Preview"
            className={styles.documentPreview}
          ></iframe>
        ) : (
          <p>No document available</p>
        )}
      </div>

      {/* Right: Extracted Content */}
      <div className={styles.extractContentSection}>
        <h3>Extracted Content</h3>
        {loading ? (
          <p>Loading...</p>
        ) : (
          renderReadableContent(data?.total_extracted_data)
        )}
        <button
          className={styles.reloadButton}
          onClick={() => handleReload("CL928697")}
          disabled={loading}
        >
          <FontAwesomeIcon
            icon={faSync}
            spin={loading}
            className={styles.icon}
          />
          {!loading && " Reload"}
        </button>
      </div>

      {/* Centered Verify Button */}
      <div className={styles.verifyButtonContainer}>
        <button
          className={styles.verifyButton}
          onClick={() => console.log("Verify action triggered")}
        >
          Verify
        </button>
      </div>
    </div>
  );
};

export default MainContent;
