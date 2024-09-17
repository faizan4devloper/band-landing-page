import React, { useState } from 'react';
import Select from 'react-select';
import styles from './Home.module.css';

const Home = () => {
    const [selectedCategory, setSelectedCategory] = useState(null);
    const [claimType, setClaimType] = useState('');
    const [claimIdVisible, setClaimIdVisible] = useState(false);
    const [selectedClaimId, setSelectedClaimId] = useState(null);
    const [uploadedDocs, setUploadedDocs] = useState([]);
    const [previewUrl, setPreviewUrl] = useState(null);

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
    ];

    const handleClaimTypeChange = (type) => {
        setClaimType(type);
        setClaimIdVisible(type === 'existing');
    };

    const handleFileUpload = (e) => {
        const file = e.target.files[0];
        if (file) {
            const newDoc = {
                name: file.name,
                type: file.type,
                file: file,
            };
            setUploadedDocs([...uploadedDocs, newDoc]);
        }
    };

    const handlePreview = (file) => {
        const url = URL.createObjectURL(file);
        setPreviewUrl(url);
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        // Add form submission logic here
        console.log('Form Submitted');
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
                            onChange={(option) => setSelectedClaimId(option)}
                            isClearable
                            className={styles.select}
                        />
                    </div>
                )}

                {/* Upload Claim Form */}
                <div className={styles.formGroup}>
                    <label htmlFor="upload">Upload Claim Form:</label>
                    <input type="file" id="upload" onChange={handleFileUpload} />
                </div>

                {/* Uploaded Documents Section */}
                {(selectedCategory && (claimType === 'new' || selectedClaimId)) && (
                    <div className={styles.uploadedDocsSection}>
                        <h3>Uploaded Docs</h3>
                        <ul className={styles.uploadedDocsList}>
                            {uploadedDocs.map((doc, index) => (
                                <li key={index}>
                                    <button 
                                        type="button" 
                                        onClick={() => handlePreview(doc.file)} 
                                        className={styles.docButton}
                                    >
                                        {doc.name}
                                    </button> - {doc.type}
                                </li>
                            ))}
                        </ul>
                    </div>
                )}

                {/* Document Preview */}
                {previewUrl && (
                    <div className={styles.previewSection}>
                        <h3>Document Preview</h3>
                        <iframe 
                            src={previewUrl} 
                            title="Document Preview" 
                            className={styles.previewIframe}
                        ></iframe>
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


/* Container */
.home {
    padding: 2rem;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    max-width: 430px;
    margin: 2rem auto;
}

/* Form Group */
.formGroup {
    margin-bottom: 1.5rem;
}

/* Form Label */
.formGroup label {
    display: block;
    font-weight: bold;
    margin-bottom: 0.5rem;    
}

/* Checkbox Group */
.checkboxGroup {
    display: flex;
    gap: 1rem;
}

/* Checkbox Label */
.checkboxGroup label {
    display: flex;
    align-items: center;
    gap: 0.5rem;
}

/* Custom Select Styling */
.select {
    margin-top: 0.5rem;
    width: 100%;
}

/* Process Button */
.processButton {
    background-color: #5f1ebe;
    color: #fff;
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.processButton:hover {
    background-color: #4725a1;
}

/* Uploaded Documents Section */
.uploadedDocsSection {
    margin-top: 2rem;
}

.uploadedDocsList {
    list-style: none;
    padding: 0;
}

.docButton {
    background: none;
    border: none;
    color: #5f1ebe;
    cursor: pointer;
    text-decoration: underline;
}

.docButton:hover {
    color: #4725a1;
}

/* Document Preview Section */
.previewSection {
    margin-top: 2rem;
}

.previewIframe {
    width: 100%;
    height: 300px;
    border: none;
}
