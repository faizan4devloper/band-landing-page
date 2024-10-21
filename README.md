
.container {
    display: grid;
    grid-template-columns: 250px 1fr 400px; /* Sidebar, FormDisplay, and UploadDocuments */
    grid-template-rows: 100vh; /* Full height */
    position: relative;
    gap: 20px; /* Adds space between sections */
    padding: 20px; /* Adds padding around the entire layout */
    box-sizing: border-box;
}

.sidebar {
    grid-column: 1; /* Sidebar on the left */
    background-color: #f8fafc;
    padding: 20px;
    box-shadow: 2px 0 10px rgba(0, 0, 0, 0.1);
    height: 100%;
}

.formDisplay {
    grid-column: 2; /* FormDisplay in the middle */
    overflow-y: auto;
    padding: 20px;
    background-color: #ffffff;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: grid-column 0.5s ease, flex 0.5s ease; /* Smooth transitions */
}

/* When UploadDocuments is visible, reduce FormDisplay width */
.reduceWidth {
    grid-column: 2 / span 1; /* Adjust FormDisplay width */
}

.uploadDocuments {
    grid-column: 3; /* UploadDocuments on the right */
    padding: 20px;
    height: 100%; /* Full height */
    background: rgba(0, 0, 0, 0.6);
    box-shadow: -2px 0 10px rgba(0, 0, 0, 0.1);
    transform: translateX(100%);
    transition: transform 0.5s ease, opacity 0.5s ease;
    opacity: 0;
    overflow-y: auto;
}

/* Slide-in from the right */
.slideIn {
    transform: translateX(0);
    opacity: 1;
}

.slideOut {
    transform: translateX(100%);
    opacity: 0;
}

.reviewSection {
    display: flex;
    flex-direction: column;
    overflow-y: auto;
    height: 100%; /* Take up full height */
    padding: 20px;
}

.preview {
    display: flex;
    flex-direction: column; /* Align previews vertically */
    gap: 15px;
}

.preview > * {
    background-color: #f1f5f9;
    border: none;
    padding: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    height: auto;
    width: 100%;
}

.togglePreviewButton {
    position: absolute;
    right: 0px;
    top: 50%;
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    color: #fff;
    border: none;
    border-radius: 6px 0px 0px 6px;
    padding: 12px 3px;
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease, transform 0.3s ease, opacity 0.3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
}

.togglePreviewButton .icon {
    transition: transform 0.5s ease;
    font-size: 1.1rem;
}

.togglePreviewButton.open .icon {
    transform: rotate(0deg); /* Rotate icon when closing */
}

.togglePreviewButton:hover {
    background-color: #0056b3;
    transform: scale(1.1); /* Slight scale effect on hover */
}

.togglePreviewButton:active {
    transform: scale(0.95); /* Shrink slightly on click */
}
