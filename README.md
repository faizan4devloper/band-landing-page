return (
  <div className={styles.container}>    
    <div className={styles.contentWrapper}>
      <div className={styles.uploadDocuments}>
        <h2 className={styles.documentHead}>Documents Review</h2>
        {allDocuments.length > 0 ? (
          <div className={styles.reviewSection}>
            <div className={styles.preview}>
              {allDocuments.map((doc, index) => (
                <div key={index} className={styles.previewItem}>
                  <p className={styles.documentType}>Type: {getDocumentType(doc)}</p>
                  {doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png') ? (
                    <img
                      src={doc.url ? doc.url : URL.createObjectURL(doc)}
                      alt={doc.name || "Document Preview"}
                      className={styles.imagePreview}
                    />
                  ) : doc.type === 'application/pdf' || doc.url?.endsWith('.pdf') ? (
                    <iframe
                      src={`${doc.url ? doc.url : URL.createObjectURL(doc)}#toolbar=0&navpanes=0&scrollbar=0`}
                      title={`PDF Preview ${index}`}
                      className={styles.pdfPreview}
                      width="550px"
                      height="750px"
                      frameBorder="0"
                    />
                  ) : doc.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' || doc.url?.endsWith('.docx') ? (
                    <a
                      href={doc.url ? doc.url : URL.createObjectURL(doc)}
                      download={doc.name || doc.name}
                      target="_blank"
                      rel="noopener noreferrer"
                      className={styles.documentLink}
                    >
                      View Document (DOCX)
                    </a>
                  ) : (
                    <a
                      href={doc.url ? doc.url : URL.createObjectURL(doc)}
                      download={doc.name || doc.name}
                      target="_blank"
                      rel="noopener noreferrer"
                      className={styles.documentLink}
                    >
                      View Document
                    </a>
                  )}
                </div>
              ))}
            </div>
          </div>
        ) : (
          <p className={styles.noFile}>No document available</p>
        )}
      </div>
      
      {/* Right side for forms */}
      <div className={styles.formsContainer}>
        <PaymentInstructionForm formData={formData} />
        <PaymentDetails paymentData={paymentData} />
        <LostPolicyForm lostPolicyFormData={lostPolicyFormData} />
        <WitnessDetails witnessData={witnessData} />
      </div>
    </div>
  </div>    
);





.container {
    display: flex;
    justify-content: center;
    padding: 3rem;
    border-radius: 12px;
}

.contentWrapper {
    display: flex;
    justify-content: space-between;
    width: 90%;
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
    border-radius: 8px;
    color: #fff;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
    padding: 2rem;
    gap: 2rem;
}

.uploadDocuments {
    flex: 1;
    max-width: 60%;
}

.formsContainer {
    flex: 1;
    display: grid;
    grid-template-columns: 1fr;
    gap: 2rem;
    background-color: #fff;
    padding: 1.5rem;
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
}

.formsContainer > div:hover {
    transform: translateY(-8px);
}

.preview {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
}

@media (max-width: 768px) {
    .contentWrapper {
        flex-direction: column;
    }

    .formsContainer {
        grid-template-columns: 1fr;
    }
}
