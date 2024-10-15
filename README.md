.container {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  max-width: 100%;
  height: 100vh;
  padding: 20px;
  position: relative;
  transition: all 0.5s ease;
  gap: 20px; /* Add space between sections */
}

.sidebar {
  flex: 0.2; /* Adjusted flex value for Sidebar */
  max-width: 220px;
  height: 100%;
  background-color: #334155;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: all 0.5s ease;
}

.formDisplay {
  flex: 0.6; /* Adjusted flex value for FormDisplay */
  height: 100%;
  background-color: #f8fafc;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  overflow-y: auto;
  transition: all 0.5s ease;
}

.uploadDocuments {
  flex: 0.8;
  background: linear-gradient(135deg, #1e293b, #334155);
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  border-radius: 12px;
  padding: 20px;
  height: 100%;
  overflow-y: auto;
  position: absolute;
  right: 0;
  top: 0;
  width: 100%;
  transform: translateX(100%);
  transition: all 0.5s ease;
  opacity: 0;
}

/* Slide-in from the right */
.slideIn {
  transform: translateX(0);
  opacity: 1;
}

/* Slide-out to the right */
.slideOut {
  transform: translateX(100%);
  opacity: 0;
}

.documentHead {
  font-size: 1.4rem;
  font-weight: bold;
  color: #fff;
  text-align: center;
}

.reviewSection {
  display: flex;
  flex-direction: column;
}

.noFile {
  color: #67748b;
  font-size: 1.2rem;
  text-align: center;
}

.preview {
  display: flex;
  margin: auto;
  flex-wrap: wrap;
  gap: 15px;
}

.preview > * {
  background-color: #e2e8f0;
  border-radius: 8px;
  padding: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.preview > *:hover {
  transform: scale(1.05);
}

.shrink {
  flex: 0.1; /* Shrink Sidebar and FormDisplay when Preview is visible */
  transition: all 0.5s ease;
}

/* Button to toggle document preview */
.togglePreviewButton {
  position: absolute;
  right: 50px;
  top: 30px;
  background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
  color: #fff;
  border: none;
  padding: 8px 18px;
  border-radius: 6px;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.togglePreviewButton:hover {
  background-color: #0056b3;
}
