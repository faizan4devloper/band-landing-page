/* Modal overlay */
.modalOverlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

/* Modal content */
.modalContent {
  position: relative;
  background: white;
  padding: 20px 20px 30px; /* Space at top for close button */
  border-radius: 10px;
  text-align: center;
  width: 600px;
  max-width: 90%;
  max-height: 90vh; /* Limit height to viewport */
  overflow-y: auto;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Close button */
.closeButton {
  position: absolute;
  top: 10px;
  right: 10px;
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #333;
  transition: color 0.3s;
}

.closeButton:hover {
  color: #572ac2;
}

/* Image preview */
.imagePreview {
  width: 100%;
  max-width: 500px;
  height: auto;
  margin-top: 20px;
}

/* Text file content display */
pre {
  white-space: pre-wrap;
  word-wrap: break-word;
  text-align: left;
  background-color: #f4f4f4;
  padding: 10px;
  border-radius: 5px;
  margin-top: 20px;
  max-height: 400px;
  overflow-y: auto;
  width: 100%;
}

/* PDF preview */
.pdfPreview {
  width: 100%;
  height: 500px;
  border: none;
  margin-top: 20px;
}
