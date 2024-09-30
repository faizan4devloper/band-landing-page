.container {
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    justify-content: space-between;
    height: 100vh; /* Full height */
    padding: 20px;
    gap: 20px; /* Add space between sidebar and form display */
}

.uploadDocuments {
    flex: 1; /* Ensure this section takes up available space */
}

.reviewSection {
    height: 100%;
    overflow-y: auto; /* Scrollable for long content */
}

/* Add this to ensure consistent form display height */
.formDisplay {
    flex: 2;
    height: 100%; /* Full height */
    overflow-y: auto; /* Allow scrolling if content overflows */
}



.sidebar {
    width: 250px; /* Fixed width for sidebar */
    background: #1e293b;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4);
    transition: all 0.3s ease;
}

.sidebar:hover {
    transform: scale(1.02);
}

.sidebarTitle {
    color: #ffffff;
    font-size: 1.8rem;
    font-weight: bold;
    margin-bottom: 20px;
    text-align: center;
}

.formList {
    list-style: none;
    padding: 0;
    margin: 0;
    max-height: 70vh; /* Allow space for scrolling if needed */
    overflow-y: auto;
}

.formItem {
    display: flex;
    align-items: center;
    padding: 15px;
    margin-bottom: 12px;
    background: #334155;
    border-radius: 8px;
    color: #e2e8f0;
    font-size: 1.2rem;
    cursor: pointer;
    transition: all 0.3s ease;
}

.formItem:hover {
    background: #475569;
    transform: translateX(5px);
}

.icon {
    margin-right: 10px;
    color: #94a3b8;
    transition: color 0.3s;
}

.formItem:hover .icon {
    color: #fbbf24;
}

.active {
    background: #64748b;
    color: #ffffff;
}

.active .icon {
    color: #fbbf24;
}
