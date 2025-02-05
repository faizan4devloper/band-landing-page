const Sidebar = ({ onFileChange, onUpload, uploading, onPolicyChange, setFileName }) => {
  const fileInputRef = useRef(null);

  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      setFileName(sanitizedFileName); // Lift state up
      onFileChange(event);
    }
  };

  return (
    <div className={styles.sidebar}>
      <input
        type="file"
        ref={fileInputRef}
        onChange={handleFileChange}
        style={{ display: "none" }}
      />
      {fileName && <p>{fileName}</p>}
    </div>
  );
};

export default Sidebar;









const MainContent = ({ message, rows, clid, setRows, staticPreviewUrl, selectedPolicy }) => {
  const [fileName, setFileName] = useState(""); // New state for file name
  const [data, setData] = useState(null);

  return (
    <div className={styles.mainContentWrapper}>
      <Sidebar setFileName={setFileName} />
      <ClaimClassification data={data} fileName={fileName} />
    </div>
  );
};

export default MainContent;











const ClaimClassification = ({ data, fileName }) => {
  const [documentName, setDocumentName] = useState('');

  useEffect(() => {
    if (fileName) {
      setDocumentName(fileName); // Prefer uploaded file name if available
    } else if (data?.total_extracted_data) {
      try {
        const parsedData = JSON.parse(data.total_extracted_data);
        setDocumentName(parsedData.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 'Unnamed Document');
      } catch (error) {
        setDocumentName('Unknown Document');
      }
    }
  }, [data, fileName]); // React when `fileName` changes

  return (
    <table>
      <thead>
        <tr>
          <th>Document Name</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>{documentName}</td>
        </tr>
      </tbody>
    </table>
  );
};

export default ClaimClassification;
