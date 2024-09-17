import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header/Header';
import Breadcrumbs from './components/Breadcrumbs/Breadcrumbs';
import WelcomeScreen from './components/WelcomeScreen/WelcomeScreen';
import Home from './components/Pages/Home'; // Create a HomeScreen component
import UploadDocuments from './components/Pages/UploadDocuments';

function App() {
    return (
        <Router>
            <div className="App">
                <Header />
                <Breadcrumbs />
                <Routes>
                    <Route path="/" element={<WelcomeScreen />} />
                    <Route path="/home" element={<Home />} />
                    <Route path="/upload-documents" element={<UploadDocuments/>}/>
                </Routes>
            </div>
        </Router>
    );
}

export default App;


import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faHome, faChevronRight } from '@fortawesome/free-solid-svg-icons';
import styles from './Breadcrumbs.module.css';

const Breadcrumbs = () => {
    const location = useLocation();
    const pathnames = location.pathname.split('/').filter((x) => x);

    // Define a mapping for path segments to breadcrumb names
    const breadcrumbNameMap = {
        home: 'Home',
        'upload-documents': 'Upload Documents',
    };

    return (
        <nav className={styles.breadcrumbs} aria-label="Breadcrumb">
            <Link to="/" className={styles.crumb}>
                <FontAwesomeIcon icon={faHome} className={styles.icon} />
            </Link>
            {pathnames.map((value, index) => {
                const to = `/${pathnames.slice(0, index + 1).join('/')}`;
                const isLast = index === pathnames.length - 1;
                const name = breadcrumbNameMap[value] || value.charAt(0).toUpperCase() + value.slice(1);

                return (
                    <span key={to} className={styles.crumb}>
                        <FontAwesomeIcon icon={faChevronRight} className={styles.separator} />
                        {isLast ? (
                            <span className={`${styles.link} ${styles.current}`} aria-current="page">
                                {name}
                            </span>
                        ) : (
                            <Link to={to} className={styles.link}>
                                {name}
                            </Link>
                        )}
                    </span>
                );
            })}
        </nav>
    );
};

export default Breadcrumbs;


import React, { useState } from 'react';
import Select from 'react-select';
import { useNavigate } from 'react-router-dom';
import styles from './Home.module.css';

// Mock function to get documents for a given claim ID
const getDocumentsForClaim = (claimId) => {
    // Replace this with actual API call to get documents
    const documents = {
        'claim-123': [{ name: 'Document 1.pdf', url: 'https://gbihr.org/images/docs/test.pdf' }],
        'claim-456': [{ name: 'Document 2.pdf', url: 'https://tourism.gov.in/sites/default/files/2019-04/dummy-pdf_2.pdf' }],
    };
    return documents[claimId] || [];
};

const Home = () => {
    const [selectedCategory, setSelectedCategory] = useState(null);
    const [claimType, setClaimType] = useState('');
    const [claimIdVisible, setClaimIdVisible] = useState(false);
    const [selectedClaimId, setSelectedClaimId] = useState(null);
    const [uploadedFile, setUploadedFile] = useState(null); // State to hold the uploaded file
    const [documents, setDocuments] = useState([]); // State to hold documents for existing claims
    const navigate = useNavigate();

    // Options for the category select input
    const categoryOptions = [
        { value: 'Surrender Claim', label: 'Surrender Claim' },
        { value: 'Retirement Claim', label: 'Retirement Claim' },
        { value: 'Death Claim', label: 'Death Claim' },
    ];

    // Options for the claim ID select input
    const claimIdOptions = [
        { value: 'claim-123', label: 'Claim ID 123' },
        { value: 'claim-456', label: 'Claim ID 456' },
        // Add more claim IDs as needed
    ];

    const handleClaimTypeChange = (type) => {
        setClaimType(type);
        setClaimIdVisible(type === 'existing');
        if (type === 'new') {
            setDocuments([]); // Reset documents for new claims
            setSelectedClaimId(null);
        }
    };

    const handleClaimIdChange = (option) => {
        setSelectedClaimId(option);
        if (option) {
            // Fetch documents for the selected claim ID
            const fetchedDocuments = getDocumentsForClaim(option.value);
            setDocuments(fetchedDocuments);
        } else {
            setDocuments([]);
        }
    };

    const handleFileChange = (e) => {
        if (e.target.files.length > 0) {
            setUploadedFile(e.target.files[0]);
        }
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        // Make sure all fields are filled before proceeding
        if (!selectedCategory || !claimType || (claimType === 'existing' && !selectedClaimId) || (claimType === 'new' && !uploadedFile)) {
            alert('Please fill in all required fields and upload a document if applicable.');
            return;
        }

        // Navigate to the Upload Documents page with the form data
        navigate('/upload-documents', {
            state: {
                selectedCategory,
                claimType,
                selectedClaimId,
                uploadedFile,
                documents,
            },
        });
    };

    return (
        <div className={styles.home}>
            <form className={styles.claimForm} onSubmit={handleSubmit}>
                {/* Choose Claim Category */}
                <div className={styles.formGroup}>
                    <label htmlFor="category">Choose Claim Category:</label>
                    <Select
                        id="category"
                        options={categoryOptions}
                        onChange={(option) => setSelectedCategory(option)}
                        isClearable
                        className={styles.select}
                    />
                </div>

                {/* Select Type of Claim */}
                <div className={styles.formGroup}>
                    <label>Select Type of Claim:</label>
                    <div className={styles.checkboxGroup}>
                        <label>
                            <input
                                type="radio"
                                name="claimType"
                                value="new"
                                checked={claimType === 'new'}
                                onChange={() => handleClaimTypeChange('new')}
                            />
                            New
                        </label>
                        <label>
                            <input
                                type="radio"
                                name="claimType"
                                value="existing"
                                checked={claimType === 'existing'}
                                onChange={() => handleClaimTypeChange('existing')}
                            />
                            Existing
                        </label>
                    </div>
                </div>

                {/* Choose Claim ID (Visible only if "Existing" is selected) */}
                {claimIdVisible && (
                    <div className={styles.formGroup}>
                        <label htmlFor="claimId">Choose Claim ID:</label>
                        <Select
                            id="claimId"
                            options={claimIdOptions}
                            onChange={handleClaimIdChange}
                            isClearable
                            className={styles.select}
                        />
                    </div>
                )}

                {/* Show associated documents for existing claims */}
                {claimType === 'existing' && documents.length > 0 && (
                    <div className={styles.formGroup}>
                        <label>Associated Documents:</label>
                        <ul className={styles.documentList}>
                            {documents.map((doc, index) => (
                                <li key={index}>
                                    <a href={doc.url} target="_blank" rel="noopener noreferrer">
                                        {doc.name}
                                    </a>
                                </li>
                            ))}
                        </ul>
                    </div>
                )}

                {/* Upload Claim Form (Visible only if "New" is selected) */}
                {claimType === 'new' && (
                    <div className={styles.formGroup}>
                        <label htmlFor="upload">Upload Claim Form:</label>
                        <input type="file" id="upload" onChange={handleFileChange} />
                    </div>
                )}

                {/* Process Button */}
                <button type="submit" className={styles.processButton}>
                    Process
                </button>
            </form>
        </div>
    );
};

export default Home;


import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents } = location.state || {};

    return (
        <div className={styles.uploadDocuments}>
            <h2>Upload Documents - Review</h2>
            <div className={styles.reviewSection}>
                <div className={styles.reviewItem}>
                    <strong>Uploaded Document:</strong> 
                    {uploadedFile ? (
                        <div className={styles.preview}>
                            {/* Show image preview if it's an image */}
                            {uploadedFile.type.startsWith('image/') && (
                                <img
                                    src={URL.createObjectURL(uploadedFile)}
                                    alt="Document Preview"
                                    className={styles.imagePreview}
                                />
                            )}
                            {/* Show PDF preview if it's a PDF */}
                            {uploadedFile.type === 'application/pdf' && (
                                <iframe
                                    src={URL.createObjectURL(uploadedFile)}
                                    title="PDF Preview"
                                    className={styles.pdfPreview}
                                    width="500px"
                                    height="500px"
                                />
                            )}
                            {/* Provide a link for other document types like DOCX */}
                            {uploadedFile.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' && (
                                <a
                                    href={URL.createObjectURL(uploadedFile)}
                                    download={uploadedFile.name}
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    className={styles.documentLink}
                                >
                                    View Document (DOCX)
                                </a>
                            )}
                            {/* Fallback link for other file types */}
                            {!uploadedFile.type.startsWith('image/') &&
                                uploadedFile.type !== 'application/pdf' &&
                                uploadedFile.type !==
                                    'application/vnd.openxmlformats-officedocument.wordprocessingml.document' && (
                                <a
                                    href={URL.createObjectURL(uploadedFile)}
                                    download={uploadedFile.name}
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    className={styles.documentLink}
                                >
                                    View Document
                                </a>
                            )}
                        </div>
                    ) : (
                        <p className={styles.noFile}>No document uploaded</p>
                    )}
                </div>

                {/* Existing Documents Preview */}
                {documents && documents.length > 0 && (
                    <div className={styles.reviewItem}>
                        <strong>Existing Documents:</strong>
                        <div className={styles.preview}>
                            {documents.map((doc, index) => (
                                <div key={index} className={styles.previewItem}>
                                    {/* Show image preview if it's an image */}
                                    {doc.url.endsWith('.jpg') || doc.url.endsWith('.png') ? (
                                        <img
                                            src={doc.url}
                                            alt={doc.name}
                                            className={styles.imagePreview}
                                        />
                                    ) : doc.url.endsWith('.pdf') ? (
                                        <iframe
                                            src={doc.url}
                                            title={`PDF Preview ${index}`}
                                            className={styles.pdfPreview}
                                            width="500px"
                                            height="500px"
                                        />
                                    ) : (
                                        <a
                                            href={doc.url}
                                            download={doc.name}
                                            target="_blank"
                                            rel="noopener noreferrer"
                                            className={styles.documentLink}
                                        >
                                            {doc.name}
                                        </a>
                                    )}
                                </div>
                            ))}
                        </div>
                    </div>
                )}
            </div>
        </div>
    );
};

export default UploadDocuments;
