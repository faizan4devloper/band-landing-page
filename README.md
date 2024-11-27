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
