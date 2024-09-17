import React, { useState } from 'react';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const [uploadedFiles, setUploadedFiles] = useState([]);

    const handleFileChange = (e) => {
        const files = Array.from(e.target.files);
        const filesWithPreview = files.map((file) => {
            const filePreview = URL.createObjectURL(file); // Create a URL for preview
            return {
                file,
                preview: filePreview,
            };
        });

        setUploadedFiles(filesWithPreview);
    };

    const handleFileRemove = (index) => {
        const newFiles = [...uploadedFiles];
        newFiles.splice(index, 1); // Remove the selected file
        setUploadedFiles(newFiles);
    };

    return (
        <div className={styles.uploadDocuments}>
            <h2>Upload Documents</h2>
            <form>
                {/* Upload Claim Form */}
                <div className={styles.formGroup}>
                    <label htmlFor="upload">Upload Claim Form:</label>
                    <input
                        type="file"
                        id="upload"
                        onChange={handleFileChange}
                        multiple
                    />
                </div>

                {/* Document Preview */}
                <div className={styles.previewContainer}>
                    {uploadedFiles.map((fileData, index) => (
                        <div key={index} className={styles.previewItem}>
                            {/* Show image preview if the file is an image */}
                            {fileData.file.type.startsWith('image/') && (
                                <img
                                    src={fileData.preview}
                                    alt="Preview"
                                    className={styles.previewImage}
                                />
                            )}
                            {/* Show a link to download or view the file if it's not an image */}
                            {!fileData.file.type.startsWith('image/') && (
                                <a
                                    href={fileData.preview}
                                    download={fileData.file.name}
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    className={styles.previewLink}
                                >
                                    {fileData.file.name}
                                </a>
                            )}
                            <button
                                type="button"
                                onClick={() => handleFileRemove(index)}
                                className={styles.removeButton}
                            >
                                Remove
                            </button>
                        </div>
                    ))}
                </div>

                {/* Process Button */}
                <button type="submit" className={styles.processButton}>
                    Process
                </button>
            </form>
        </div>
    );
};

export default UploadDocuments;

/* Styles for the preview section */
.previewContainer {
    margin-top: 1rem;
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}

.previewItem {
    position: relative;
    border: 1px solid #ccc;
    padding: 0.5rem;
    border-radius: 5px;
    background-color: #f9f9f9;
}

.previewImage {
    max-width: 100px;
    max-height: 100px;
    object-fit: cover;
}

.previewLink {
    display: block;
    margin-top: 0.5rem;
    color: #007bff;
    text-decoration: underline;
}

.removeButton {
    position: absolute;
    top: 0.5rem;
    right: 0.5rem;
    background-color: red;
    color: white;
    border: none;
    border-radius: 50%;
    width: 20px;
    height: 20px;
    cursor: pointer;
}
