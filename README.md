/* Sidebar Container */
.sidebar {
  width: 250px;
  height: 100vh;
  background: #ffffff;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  transition: width 0.3s ease-in-out;
}

/* Hidden Sidebar */
.sidebarHidden {
  width: 0;
  overflow: hidden;
  padding: 0;
  transition: width 0.3s ease-in-out;
}

/* Toggle Button */
.toggleButton {
  position: absolute;
  top: 50%;
  right: -15px;
  transform: translateY(-50%);
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1);
  border: none;
  width: 35px;
  height: 35px;
  cursor: pointer;
  font-size: 15px;
  color: #fff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
  transition: all 0.3s ease;
}

.toggleButton:hover {
  background: #0f5fdc;
}

/* Heading */
.heading {
  font-size: 1.2rem;
  font-weight: bold;
  color: #0f5fdc;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  margin-bottom: 20px;
}

/* Dropzone */
.fileInputContainer {
  border: 2px dashed #ccc;
  padding: 15px;
  border-radius: 10px;
  font-size: 1rem;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
  width: 100%;
}

.fileInputContainer:hover {
  background-color: #e8f5e9;
  border-color: #4caf50;
}

/* File Name */
.fileNameContainer {
  margin-top: 10px;
  padding: 10px;
  width: 100%;
  text-align: center;
  border-radius: 8px;
  background: #f3f3f3;
}

.fileName {
  font-size: 14px;
  color: #333;
  font-weight: 600;
  word-wrap: break-word;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

/* Upload Button */
.uploadButton {
  padding: 10px 20px;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
  transition: all 0.3s ease;
}

.uploadButton:disabled {
  background: #cccccc;
  cursor: not-allowed;
}

.uploadButton:hover:not(:disabled) {
  background-color: #4caf50;
}

/* Loader */
.loader {
  border: 3px solid #f3f3f3;
  border-top: 3px solid #4CAF50;
  border-radius: 50%;
  width: 16px;
  height: 16px;
  animation: spin 1s linear infinite;
}

/* Loader Animation */
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/* Success Message */
.successMessage {
  font-size: 14px;
  color: #28a745;
  margin-top: 10px;
  font-weight: bold;
}
