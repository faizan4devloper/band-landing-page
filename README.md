{isUploaded ? (
          <MainContent 
            message={message} 
            selectedPolicy={selectedPolicy}
            rows={rows} 
            clid={rows[0].recNum}
            setRows={setRows} 
            staticPreviewUrl={previewUrl} 
            uploadedFileName={uploadedFileName}
          />
        ) : (
          <p className={styles.infoMessage}>
            Please upload a document to view the data.
          </p>
        )}
