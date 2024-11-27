 <div className={styles.previewSection}>
        <h3>Document Preview</h3>
        {uploadedDocumentUrl ? (
          <iframe
            src={uploadedDocumentUrl}
            title="Document Preview"
            className={styles.documentPreviewIframe}
          ></iframe>
        ) : (
          <p>No document uploaded yet.</p>
        )}
      </div>
