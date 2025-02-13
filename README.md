/* Sidebar.module.css - Enhanced */
.sidebar {
  width: 320px;
  background: linear-gradient(195deg, #ffffff 0%, #f8fafc 100%);
  border-right: 1px solid #e2e8f0;
  padding: 1.5rem;
  height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  box-shadow: 4px 0 6px -1px rgba(0, 0, 0, 0.05);
  position: relative;
}

.heading {
  font-size: 1.5rem;
  font-weight: 600;
  color: #1e293b;
  margin: 0;
  padding-top: 1rem;
}

.fileInputContainer {
  border: 2px dashed #cbd5e1;
  border-radius: 0.75rem;
  padding: 2rem 1rem;
  text-align: center;
  cursor: pointer;
  transition: all 0.2s ease;
  background: #ffffff;
}

.fileInputContainer:hover {
  border-color: #3b82f6;
  background: #f8fafc;
}

.dropzoneText {
  color: #64748b;
  font-size: 0.875rem;
  margin: 0;
  pointer-events: none;
}

.fileNameContainer {
  background: #ffffff;
  border-radius: 0.75rem;
  padding: 1rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
}

.fileName {
  color: #1e293b;
  font-size: 0.875rem;
  font-weight: 500;
  margin: 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.uploadButton {
  background: #3b82f6;
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 0.75rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  width: 100%;
}

.uploadButton:hover:not(:disabled) {
  background: #2563eb;
  transform: translateY(-1px);
}

.uploadButton:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.loader {
  border: 3px solid #f3f3f3;
  border-top: 3px solid #3b82f6;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  animation: spin 1s linear infinite;
}

.closeSidebarButton {
  position: absolute;
  top: 1rem;
  left: 1rem;
  background: #3b82f6;
  color: white;
  border: none;
  padding: 0.5rem;
  border-radius: 0.5rem;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Responsive Design */
@media (max-width: 768px) {
  .sidebar {
    width: 100%;
    padding: 1rem;
  }
  
  .heading {
    font-size: 1.25rem;
  }
  
  .fileInputContainer {
    padding: 1.5rem 1rem;
  }
}
