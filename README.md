/* Sidebar container */
.sidebar {
    background-color: #f1f5f9;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    height: 100%; /* Full height as defined by parent */
    display: flex;
    flex-direction: column;
    gap: 15px; /* Space between items */
    overflow-y: auto; /* Allow scrolling if necessary */
}

/* Sidebar heading */
.sidebarHead {
    font-size: 1.5rem;
    font-weight: bold;
    color: #1e293b;
    text-align: center;
    margin-bottom: 15px;
}

/* Sidebar list */
.sidebarList {
    list-style: none;
    padding: 0;
    margin: 0;
}

/* Sidebar list item */
.sidebarListItem {
    padding: 12px 15px;
    background-color: #e2e8f0;
    border-radius: 8px;
    font-size: 1.2rem;
    color: #334155;
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

/* Hover effect for sidebar items */
.sidebarListItem:hover {
    background-color: #cbd5e1;
    transform: scale(1.02);
}

/* Active sidebar item */
.activeItem {
    background-color: #7ca2e1;
    color: #ffffff;
}

/* Right arrow icon */
.sidebarIcon {
    font-size: 1.2rem;
    color: #64748b;
}

/* Add extra spacing if needed for large lists */
@media screen and (max-width: 768px) {
    .sidebar {
        height: auto; /* In case of mobile, allow the sidebar to adapt */
    }
}







/* Form display container */
.formDisplay {
    background-color: #ffffff;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.1);
    height: 100%; /* Full height as defined by parent */
    overflow-y: auto; /* Scroll if form data exceeds container height */
    display: flex;
    flex-direction: column;
    gap: 20px; /* Space between form fields or content */
}

/* Form heading */
.formHead {
    font-size: 1.6rem;
    font-weight: bold;
    color: #1e293b;
    text-align: center;
    margin-bottom: 10px;
}

/* Form field group */
.formGroup {
    display: flex;
    flex-direction: column;
    gap: 10px; /* Space between input fields */
}

/* Form labels */
.formLabel {
    font-size: 1.2rem;
    color: #475569;
    font-weight: 500;
}

/* Form input field */
.formInput {
    padding: 12px;
    font-size: 1.1rem;
    color: #334155;
    background-color: #f1f5f9;
    border: 1px solid #e2e8f0;
    border-radius: 8px;
    transition: all 0.3s ease;
}

/* Form input field on focus */
.formInput:focus {
    border-color: #7ca2e1;
    outline: none;
    background-color: #ffffff;
    box-shadow: 0 0 0 3px rgba(124, 162, 225, 0.2);
}

/* Button styling */
.formButton {
    padding: 12px 20px;
    font-size: 1.2rem;
    font-weight: bold;
    color: #ffffff;
    background-color: #3b82f6;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    margin-top: 10px;
}

/* Button hover */
.formButton:hover {
    background-color: #2563eb;
}

/* Button disabled state */
.formButton:disabled {
    background-color: #94a3b8;
    cursor: not-allowed;
}
