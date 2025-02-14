import React from 'react';
import styles from "./DocumentPreview.module.css";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faCloudUploadAlt, 
  faFilePdf, 
  faExpand, 
  faDownload 
} from '@fortawesome/free-solid-svg-icons';

const DocumentPreview = ({ staticPreviewUrl, onUpload }) => {
  return (
    <div className={styles.previewContainer}>
      <div className={styles.previewHeader}>
        <h3 className={styles.title}>
          <span className={styles.titleIcon}>📄</span>
          Document Viewer
        </h3>
        
        {staticPreviewUrl && (
          <div className={styles.controls}>
            <button className={styles.controlButton}>
              <FontAwesomeIcon icon={faDownload} />
            </button>
            <button className={styles.controlButton}>
              <FontAwesomeIcon icon={faExpand} />
            </button>
          </div>
        )}
      </div>

      <div className={styles.previewContent}>
        {staticPreviewUrl ? (
          <>
            <div className={styles.iframeContainer}>
              <iframe
                src={staticPreviewUrl}
                className={styles.documentFrame}
              />
            </div>
            <div className={styles.loadingOverlay}>
              <div className={styles.loadingBar} />
            </div>
          </>
        ) : (
          <div className={styles.uploadPrompt}>
            <div className={styles.uploadVisual}>
              <FontAwesomeIcon 
                icon={faFilePdf} 
                className={styles.pdfIcon}
              />
              <div className={styles.uploadDashedBorder} />
            </div>
            <button 
              className={styles.uploadButton}
              onClick={onUpload}
            >
              <FontAwesomeIcon icon={faCloudUploadAlt} />
              Select PDF Document
            </button>
            <p className={styles.uploadHint}>
              Supported format: PDF (max 25MB)
            </p>
          </div>
        )}
      </div>
    </div>
  );
};

export default DocumentPreview;


/* DocumentPreview.module.css */
.previewContainer {
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.08);
  overflow: hidden;
  transition: transform 0.2s ease;
}

.previewContainer:hover {
  transform: translateY(-2px);
}

.previewHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.25rem;
  background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
  border-bottom: 1px solid #e2e8f0;
}

.title {
  margin: 0;
  font-size: 1.1rem;
  color: #1e293b;
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.titleIcon {
  font-size: 1.4em;
}

.controls {
  display: flex;
  gap: 0.5rem;
}

.controlButton {
  background: none;
  border: 1px solid #cbd5e1;
  color: #64748b;
  width: 36px;
  height: 36px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.controlButton:hover {
  background: #ffffff;
  color: #0f5fdc;
  border-color: #bfdbfe;
}

.previewContent {
  height: 500px;
  position: relative;
}

.iframeContainer {
  height: 100%;
  background: #f8fafc;
}

.documentFrame {
  width: 100%;
  height: 100%;
  border: none;
}

.loadingOverlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  overflow: hidden;
}

.loadingBar {
  height: 100%;
  width: 45%;
  background: linear-gradient(90deg, transparent 0%, #0f5fdc 50%, transparent 100%);
  animation: loading 1.5s infinite;
}

.uploadPrompt {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}

.uploadVisual {
  position: relative;
  margin-bottom: 2rem;
}

.pdfIcon {
  font-size: 4rem;
  color: #0f5fdc;
  opacity: 0.8;
  z-index: 1;
  position: relative;
}

.uploadDashedBorder {
  position: absolute;
  width: 120px;
  height: 120px;
  border: 2px dashed #cbd5e1;
  border-radius: 50%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: pulse 2s infinite;
}

.uploadButton {
  background: #0f5fdc;
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 8px;
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.uploadButton:hover {
  background: #1e40af;
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(15, 95, 220, 0.25);
}

.uploadHint {
  color: #64748b;
  font-size: 0.9rem;
  margin-top: 1rem;
}

@keyframes loading {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(200%); }
}

@keyframes pulse {
  0% { opacity: 0.6; transform: translate(-50%, -50%) scale(1); }
  50% { opacity: 1; transform: translate(-50%, -50%) scale(1.05); }
  100% { opacity: 0.6; transform: translate(-50%, -50%) scale(1); }
}


import React from 'react';
import styles from "./ExtractedContent.module.css";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faSync } from '@fortawesome/free-solid-svg-icons';

const CollapsibleSection = ({ title, children, isFirst }) => {
  const [isOpen, setIsOpen] = React.useState(!isFirst);

  return (
    <div className={styles.section}>
      <button 
        className={styles.sectionHeader}
        onClick={() => setIsOpen(!isOpen)}
      >
        <span className={styles.sectionTitle}>{title}</span>
        <FontAwesomeIcon 
          icon={faChevronDown} 
          className={`${styles.chevron} ${isOpen ? styles.open : ''}`}
        />
      </button>
      {isOpen && (
        <div className={styles.sectionContent}>
          {children}
        </div>
      )}
    </div>
  );
};

const ExtractedContent = ({ data, loading, handleReload }) => {
  const parsedData = data ? JSON.parse(data.total_extracted_data) : null;

  return (
    <div className={styles.container}>
      <div className={styles.header}>
        <h3 className={styles.title}>Data Extraction</h3>
        <button 
          className={styles.reloadButton}
          onClick={handleReload}
          disabled={loading}
        >
          <FontAwesomeIcon 
            icon={faSync} 
            className={loading ? styles.spin : ''}
          />
          {!loading && 'Refresh Data'}
        </button>
      </div>

      <div className={styles.content}>
        {!parsedData ? (
          <div className={styles.emptyState}>
            <div className={styles.loadingBar} />
            <div className={styles.loadingBar} />
            <div className={styles.loadingBar} />
          </div>
        ) : (
          Object.keys(parsedData).map((sectionKey, index) => (
            <CollapsibleSection 
              key={sectionKey}
              title={sectionKey.replace(/_/g, ' ')}
              isFirst={index === 0}
            >
              <div className={styles.sectionGrid}>
                {Object.entries(parsedData[sectionKey]).map(([key, value]) => (
                  <div key={key} className={styles.dataItem}>
                    <label>{key.replace(/_/g, ' ')}</label>
                    <div className={styles.value}>
                      {value || <span className={styles.nullValue}>N/A</span>}
                    </div>
                  </div>
                ))}
              </div>
            </CollapsibleSection>
          ))
        )}
      </div>
    </div>
  );
};

export default ExtractedContent;


/* ExtractedContent.module.css */
.container {
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.08);
  overflow: hidden;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.25rem;
  background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
  border-bottom: 1px solid #e2e8f0;
}

.title {
  margin: 0;
  font-size: 1.1rem;
  color: #1e293b;
}

.reloadButton {
  background: none;
  border: 1px solid #cbd5e1;
  color: #64748b;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.reloadButton:hover {
  background: #ffffff;
  color: #0f5fdc;
  border-color: #bfdbfe;
}

.content {
  padding: 1rem;
  max-height: 600px;
  overflow-y: auto;
}

.section {
  margin-bottom: 0.5rem;
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}

.sectionHeader {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: #f8fafc;
  border: none;
  cursor: pointer;
  transition: background 0.2s ease;
}

.sectionHeader:hover {
  background: #f1f5f9;
}

.sectionTitle {
  font-weight: 500;
  color: #1e293b;
  text-transform: capitalize;
}

.chevron {
  transition: transform 0.2s ease;
  color: #94a3b8;
}

.chevron.open {
  transform: rotate(180deg);
}

.sectionContent {
  padding: 1rem;
}

.sectionGrid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1rem;
}

.dataItem {
  padding: 0.75rem;
  background: #f8fafc;
  border-radius: 6px;
}

.dataItem label {
  display: block;
  font-size: 0.85rem;
  color: #64748b;
  margin-bottom: 0.25rem;
}

.value {
  font-weight: 500;
  color: #1e293b;
  word-break: break-word;
}

.nullValue {
  color: #94a3b8;
  font-style: italic;
}

.emptyState {
  padding: 1rem;
  display: grid;
  gap: 0.75rem;
}

.loadingBar {
  height: 40px;
  background: linear-gradient(90deg, #f1f5f9 25%, #e2e8f0 50%, #f1f5f9 75%);
  background-size: 200% 100%;
  border-radius: 6px;
  animation: loading 1.5s infinite linear;
}

.spin {
  animation: spin 1s infinite linear;
}

@keyframes loading {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
