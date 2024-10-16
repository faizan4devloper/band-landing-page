/* Container must be flex */
.container {
  display: flex;
  width: 100%;
  height: 100vh;
  transition: all 0.5s ease;
}

/* Shrinking effect for Sidebar */
.shrinkSidebar {
  flex-basis: 250px; /* Shrinks Sidebar when preview is open */
  transition: flex-basis 0.5s ease;
}

.sidebarNormal {
  flex-basis: 330px; /* Normal width for Sidebar */
  transition: flex-basis 0.5s ease;
}

/* Shrinking effect for FormDisplay */
.shrinkFormDisplay {
  flex-basis: 30%; /* 30% width for FormDisplay when preview is open */
  transition: flex-basis 0.5s ease;
}

.formDisplayNormal {
  flex-basis: 100%; /* 70% width for FormDisplay when preview is closed */
  transition: flex-basis 0.5s ease;
}

/* UploadDocuments section takes remaining space */
.uploadDocuments {
  flex-grow: 1;
  background: linear-gradient(135deg, #1e293b, #334155);
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  border-radius: 12px;
  padding: 20px;
  height: 100%;
  overflow-y: auto;
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

.preview {
  display: flex;
  margin: auto;
  flex-wrap: wrap;
  gap: 15px;
}

.noFile {
  color: #67748b;
  font-size: 1.2rem;
  text-align: center;
}

/* Toggle Button */
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
