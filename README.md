:root {
  --primary-color: #6a11cb;
  --secondary-color: #007bff;
  --background-color: #f4f4f4;
  --button-text-color: #ffffff;
  --modal-background: #ffffff;
  --modal-overlay: rgba(0, 0, 0, 0.7);
  --border-color: #ddd;
  --loading-icon-color: #666;
  --hover-bg-color: #e2e2e2;
  --disabled-bg-color: #cccccc;
  --text-color: #333;
  --empty-bg-color: #f9f9f9;
}

body {
  font-family: 'Roboto', sans-serif;
  background-color: var(--background-color);
  margin: 0;
  padding: 0;
}

.documentTableContainer {
  background-color: var(--modal-background);
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.tableHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.tableHeader h3 {
  font-size: 24px;
  font-weight: 600;
  color: var(--text-color);
}

.headerActions {
  display: flex;
  align-items: center;
}

.refreshButton {
  background-color: var(--primary-color);
  color: var(--button-text-color);
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.refreshButton:hover {
  background-color: var(--secondary-color);
}

.refreshButton:disabled {
  background-color: var(--disabled-bg-color);
  cursor: not-allowed;
}

.documentTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.documentTable th,
.documentTable td {
  border: 1px solid var(--border-color);
  padding: 12px 16px;
  text-align: left;
  font-size: 14px;
  color: var(--text-color);
}

.documentTable th {
  background-color: #f4f4f4;
  font-weight: 600;
}

.documentTable tr:nth-child(even) {
  background-color: var(--empty-bg-color);
}

.documentTable tr:hover {
  background-color: var(--hover-bg-color);
}

.documentRow td {
  font-size: 14px;
}

.actionButtons {
  display: flex;
  gap: 12px;
}

.viewButton, .downloadButton {
  background-color: var(--primary-color);
  color: var(--button-text-color);
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.viewButton:hover, .downloadButton:hover {
  background-color: var(--secondary-color);
}

.seeMoreButton {
  background-color: var(--primary-color);
  color: var(--button-text-color);
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.seeMoreButton:hover {
  background-color: var(--secondary-color);
}

.emptyRow {
  text-align: center;
}

.emptyContent {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 20px;
}

.emptyIcon {
  font-size: 48px;
  color: #ccc;
}

.loadingRow {
  text-align: center;
}

.loadingContent {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 20px;
}

.loadingIcon {
  font-size: 36px;
  margin-right: 8px;
  color: var(--loading-icon-color);
}

.pagination {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
}

.pagination button {
  background-color: var(--secondary-color);
  color: var(--button-text-color);
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.pagination button:disabled {
  background-color: var(--disabled-bg-color);
  cursor: not-allowed;
}

.pagination button:hover {
  background-color: var(--primary-color);
}

.modalContainer {
  outline: none;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: var(--modal-overlay);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modalContent {
  display: flex;
  width: 100%;
  height: 560px;
  background-color: var(--modal-background);
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.previewSection {
  flex: 3;
  background-color: #f4f4f4;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.previewWrapper {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.previewIframe {
  width: 95%;
  height: 95%;
  border: none;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.metadataSection {
  flex: 2;
  display: flex;
  flex-direction: column;
  padding: 30px;
  background-color: var(--modal-background);
  overflow: hidden;
}

.metadataHeader {
  text-align: center;
  margin-bottom: 20px;
  border-bottom: 2px solid #f0f0f0;
  padding-bottom: 15px;
}

.documentTitle {
  font-size: 22px;
  font-weight: 600;
  color: var(--text-color);
  margin: 0;
}

.metadataContent {
  flex-grow: 1;
  overflow-y: auto;
  background-color: #f9f9f9;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
}

.metadataContent h3 {
  font-size: 18px;
  color: var(--primary-color);
  margin-bottom: 15px;
}

.metadataJson {
  background-color: #f0f0f0;
  color: var(--text-color);
  font-size: 14px;
  font-family: 'Courier New', monospace;
  padding: 15px;
  border-radius: 8px;
  white-space: pre-wrap;
  overflow-wrap: break-word;
  max-height: 300px;
  overflow-y: auto;
}

.modalActions {
  display: flex;
  justify-content: center;
}

.closeModalButton {
  background-color: #e53935;
  position: absolute;
  top: 12px;
  right: 131px;
  cursor: pointer;
  color: var(--button-text-color);
  border: none;
  padding: 9px 19px;
  border-radius: 0px 16px 0px 0px;
  font-size: 16px;
  transition: all 0.3s ease;
}

.closeModalButton:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

/* Responsive Adjustments */
@media (max-width: 768px) {
  .documentTable th, .documentTable td {
    font-size: 12px;
    padding: 8px;
  }

  .documentTable tr {
    display: block;
    margin-bottom: 12px;
  }

  .actionButtons {
    justify-content: space-between;
  }

  .previewSection,
  .metadataSection {
    flex: 1 100%;
    width: 100%;
  }

  .modalContent {
    flex-direction: column;
    width: 95%;
    height: 90vh;
  }

  .previewWrapper {
    width: 100%;
    height: 100%;
  }

  .modalActions {
    margin-top: 20px;
  }
}
