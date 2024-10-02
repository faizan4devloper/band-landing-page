/* Main container for the entire UploadDocuments page */
.container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-start;
    max-width: 100%;
    gap: 20px; /* Space between sidebar, form, and documents */
    padding: 20px;
    /* Remove height constraint to allow the content to determine the container's size */
}

/* Document review section styling */
.uploadDocuments {
    flex-grow: 2; /* Take up more space but allow flex to control the height naturally */
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
    border-radius: 12px;
    padding: 20px;
    /* Remove fixed height to prevent the bottom constraint */
    overflow-y: auto; /* Scroll if content exceeds the container height */
}

/* Headings */
.documentHead {
    font-size: 1.8rem;
    font-weight: bold;
    margin-bottom: 20px;
    color: #fff;
    text-align: center;
}

/* Review section for documents */
.reviewSection {
    display: flex;
    flex-direction: column;
}

/* No file message */
.noFile {
    color: #64748b;
    font-size: 1.2rem;
    text-align: center;
}

/* Preview section for individual documents */
.preview {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
}

/* Individual document preview styling */
.preview > * {
    background-color: #e2e8f0;
    border-radius: 8px;
    padding: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
}

/* When hovering over individual document preview */
.preview > *:hover {
    transform: scale(1.05);
}

/* Sidebar styling */
.sidebar {
    flex-grow: 1;
    max-width: 250px;
    height: auto; /* Adjust height to fit content */
}

/* FormDisplay styling */
.formDisplay {
    flex-grow: 3; /* Form takes up more space but is flexible */
    height: auto; /* Adjust height to fit content */
    max-width: 600px; /* Optional, to limit form width */
    overflow-y: auto; /* Scroll if content exceeds height */
}
