/* Main container */
.formDisplay {
    background: linear-gradient(135deg, #1e293b, #334155);
    padding: 30px;
    height: 100%;
    width: 100%;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 20px;
    font-family: 'Poppins', sans-serif;
}

/* Loading and select messages */
.loadingMessage,
.selectMessage {
    font-size: 1.8rem;
    color: #e2e8f0;
    text-align: center;
    margin-top: 20px;
    font-weight: 500;
}

/* Form heading */
.formHead {
    font-size: 2rem;
    font-weight: bold;
    color: #f8fafc;
    text-align: left;
    margin-bottom: 25px;
    text-transform: capitalize;
    display: flex;
    align-items: center;
}

.headerIcon {
    margin-right: 10px;
    color: #9dc2ff;
}

/* Table container */
.tableContainer {
    margin-top: 20px;
    width: 100%;
    background-color: rgba(255, 255, 255, 0.1); /* Glass effect */
    border-radius: 12px;
    padding: 20px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
    backdrop-filter: blur(10px);
    overflow-x: auto;
    position: relative;
}

/* Table styles */
.dataTable {
    width: 100%;
    border-collapse: collapse;
    background-color: transparent;
}

/* Table headers */
.tableHeader {
    font-size: 1.1rem;
    font-weight: bold;
    color: #f1f5f9;
    background-color: transparent;
    padding: 16px 12px;
    text-align: left;
    border-bottom: 2px solid rgba(255, 255, 255, 0.2);
}

/* Table rows */
.tableRow {
    transition: background-color 0.4s ease, transform 0.3s ease;
    cursor: pointer;
}

.tableRow:hover {
    background-color: rgba(255, 255, 255, 0.05);
    transform: scale(1.02);
}

/* Table cells */
.tableCell {
    font-size: 1rem;
    font-weight: 500;
    color: #cbd5e1;
    padding: 16px 12px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
    display: flex;
    align-items: center;
}

/* Icons in the table */
.tableIcon {
    margin-right: 8px;
    color: #9dc2ff;
    transition: color 0.3s ease;
}

.tableIcon:hover {
    color: #fbbf24; /* On hover, change to yellow */
}

/* Heading cell (first column) */
.tableHeadingCell {
    font-size: 1.3rem;  /* Larger font size */
    font-weight: bold;  /* Bold text for heading */
    color: #7dd3fc; /* Light blue */
    padding: 16px 12px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
    display: flex;
    align-items: center;
    text-transform: capitalize;
}

/* Next button */
.nextBtn {
    position: absolute;
    top: -40px;
    right: 20px;
    padding: 0.8rem 2.5rem;
    font-size: 1.2rem;
    color: #fff;
    background: linear-gradient(135deg, #7ca2e1, #2563eb);
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.4);
}

.nextBtn:hover {
    background: linear-gradient(135deg, #f2f2f2 -20%, #7ca2e1);
    transform: scale(1.05);
}

.nextIcon {
    margin-left: 10px;
    transition: transform 0.3s ease;
}

.nextBtn:hover .nextIcon {
    transform: translateX(8px);
}

/* Expandable Row */
.expandableRow {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.5s ease-out, padding 0.5s ease-out;
}

.expandableRow.expanded {
    max-height: 300px; /* Adjust based on content */
    padding: 10px 0;
}

/* Responsive design */
@media (max-width: 768px) {
    .tableCell {
        white-space: normal;
    }

    .formHead {
        font-size: 1.7rem;
    }

    .nextBtn {
        top: -30px;
        right: 15px;
    }
}






const NewFormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
    const [expandedRows, setExpandedRows] = useState([]);
    const navigate = useNavigate(); 

    useEffect(() => {
        setFormData(formDataJson); 
    }, []);

    const handleNextClick = () => {
        navigate('/New-page');
    };

    const chooseIcon = (key) => {
        switch (key.toLowerCase()) {
            case 'approver1_focuses_on': return faInfoCircle;
            case 'approver2_focuses_on': return faSignature;
            case 'additional_information': return faFileAlt;
            case 'suggested_action_items': return faQuestionCircle;
            case 'detailed_summary': return faIdCard;
            default: return faCalendarAlt;
        }
    };

    const toggleExpandRow = (key) => {
        setExpandedRows((prevState) =>
            prevState.includes(key) ? prevState.filter(row => row !== key) : [...prevState, key]
        );
    };

    const renderFormData = () => {
        if (!formData) {
            return <p className={styles.loadingMessage}>Loading data...</p>;
        }

        const selectedData = formData[selectedForm];
        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div className={styles.tableContainer}>
                <button className={styles.nextBtn} onClick={handleNextClick}>
                    Next <FontAwesomeIcon icon={faChevronRight} className={styles.nextIcon} />
                </button>

                <table className={styles.dataTable}>
                    <tbody>
                        {Object.entries(selectedData).map(([key, value]) => (
                            <React.Fragment key={key}>
                                <tr className={styles.tableRow} onClick={() => toggleExpandRow(key)}>
                                    <td className={styles.tableHeadingCell}>
                                        <FontAwesomeIcon icon={chooseIcon(key)} className={styles.tableIcon} />
                                        {key.replace(/_/g, ' ')}
                                    </td>
                                    <td className={styles.tableCell}>
                                        {Array.isArray(value) ? value.join(', ') : value}
                                    </td>
                                </tr>
                                <tr className={`${styles.expandableRow} ${expandedRows.includes(key) ? styles.expanded : ''}`}>
                                    <td colSpan="2"> {/* Add more details or content here */} Expanded content for {key}</td>
                                </tr>
                            </React.Fragment>
                        ))}
                    </tbody>
                </table>
            </div>
        );
    };

    return (
        <div className={styles.formDisplay}>
            {renderFormData()}
        </div>
    );
};

export default NewFormDisplay;
