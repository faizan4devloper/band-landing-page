{/* Upload Area (Drag and Drop) */}
                {claimType === 'new' && (
                    <div className={styles.formGroup}>
                        <label>Upload Claim Form:</label>
                        <div {...getRootProps({ className: styles.dropzone })}>
                            <input {...getInputProps()} />
                            {isDragActive ? (
                                <p>Drop the files here...</p>
                            ) : (
                                <p><FontAwesomeIcon icon={faUpload} /></p>
                                <p>Drag & Drop or Choose File to Upload</p>
                            )}
                        </div>
                        {uploadedFile && <p className={styles.uploadedFileName}>{uploadedFile.name}</p>}
                    </div>
                )}
