/* TableData.module.css */
.documentTableContainer {
  background-color: #ffffff;
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
  color: #333;
}

.headerActions {
  display: flex;
  align-items: center;
}

.refreshButton {
  background-color: #4a90e2;
  color: white;
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
  background-color: #0056b3;
}

.refreshButton:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.documentTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.documentTable th,
.documentTable td {
  border: 1px solid #ddd;
  padding: 12px 16px;
  text-align: left;
  font-size: 14px;
  color: #333;
}

.documentTable th {
  background-color: #f4f4f4;
  font-weight: 600;
}

.documentTable tr:nth-child(even) {
  background-color: #f9f9f9;
}

.documentTable tr:hover {
  background-color: #e2e2e2;
}

.documentRow td {
  font-size: 14px;
}

.actionButtons {
  display: flex;
  gap: 12px;
}

.viewButton, .downloadButton {
  background-color: #4a90e2;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.viewButton:hover, .downloadButton:hover {
  background-color: #0056b3;
}

.seeMoreButton {
  background-color: #4a90e2;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.seeMoreButton:hover {
  background-color: #0056b3;
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
}

.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
  margin-top: 20px;
  background-color: #f4f6f9;
  padding: 15px;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.paginationButton {
  position: relative;
  display: flex;
  text-transform: capitalize;
  align-items: center;
  font-size: 12px;
  justify-content: center;
  padding: 8px 10px;
  background-color: #4a90e2;
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  letter-spacing: 0.5px;
  cursor: pointer;
  transition: all 0.3s ease;
  overflow: hidden;
  box-shadow: 0 3px 6px rgba(74, 144, 226, 0.3);
}

.paginationButton::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(120deg, transparent, rgba(255, 255, 255, 0.3), transparent);
  transition: all 0.6s ease;
}

.paginationButton:hover::before {
  left: 100%;
}

.paginationButton:hover {
  transform: translateY(-3px);
  box-shadow: 0 5px 10px rgba(74, 144, 226, 0.4);
}

.paginationButton:active {
  transform: translateY(1px);
  box-shadow: 0 2px 4px rgba(74, 144, 226, 0.2);
}

.paginationButton.disabled {
  background-color: #a0a0a0;
  cursor: not-allowed;
  pointer-events: none;
  box-shadow: none;
}

.paginationButton.disabled:hover {
  transform: none;
}

.paginationInfo {
  display: flex;
  align-items: center;
  background-color: #f0f4f8;
  padding: 8px 15px;
  border-radius: 6px;
  font-weight: 500;
  color: #4a90e2;
  min-width: 120px;
  justify-content: center;
}

.paginationIcon {
  margin-right: 5px;
  margin-left: 5px;
  font-size: 14px;
}

.paginationContainer {
  display: flex;
  align-items: center;
  gap: 15px;
}

@keyframes buttonPulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}

.paginationButton:hover {
  animation: buttonPulse 0.7s infinite;
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
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

#toolbar {
  align-items: center;
  background-color: var(--viewer-pdf-toolbar-background-color);
  color: rgb(255, 255, 255);
  display: flex;
  padding: 0px 16px;
  display: none;
}

.modalContent {
  display: flex;
  width: 100%;
  height: 560px;
  background-color: #ffffff;
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
  background-color: #ffffff;
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
  color: #333;
  margin: 0;
}

.metadataContent {
  flex-grow: 1;
  overflow-y: auto;
  background-color: #f9f9f9;
  border-radius: 10px;
}
