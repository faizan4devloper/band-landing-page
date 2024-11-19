<div className={styles.fileInputContainer}>
        <label htmlFor="fileUpload" className={styles.fileLabel}>
          Choose File
          <input
            type="file"
            id="fileUpload"
            onChange={onFileChange}
            className={styles.fileInput}
          />
        </label>
      </div>
