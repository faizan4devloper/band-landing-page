      background: linear-gradient(135deg, #F2F2F2 0%, #7ca2e1 100%);
this is my whole web app background color and header color is
background: rgba(0, 0, 0, 0.6);
i want suitable color of sidebar


/* Container styling */
.sidebar {
    background-color: #1e293b;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    height: 100%; /* Full height as defined by parent */
    display: flex;
    flex-direction: column;
    gap: 15px; /* Space between items */
    overflow-y: auto; /* Allow scrolling if necessary */}

.sidebar:hover {
    transform: scale(1.02); /* Subtle scaling on hover */
}

/* Sidebar title */
.sidebarTitle {
    color: #ffffff; /* White title for contrast */
    font-size: 1.8rem;
    font-weight: bold;
    margin-bottom: 20px;
    text-align: center;
}

/* Form list */
.formList {
    list-style: none;
    padding: 0;
    margin: 0;
}

/* Form items */
.formItem {
    display: flex;
    align-items: center;
    padding: 12px;
    margin-bottom: 12px;
    background: #334155; /* Darker item background */
    border-radius: 8px;
    color: #e2e8f0; /* Light text color */
    font-size: 1.1rem;
    cursor: pointer;
    transition: all 0.3s ease;
}

.formItem:hover {
    background: #475569; /* Lighter on hover */
    transform: translateX(5px); /* Slight movement on hover */
}

.icon {
    margin-right: 10px;
    color: #94a3b8; /* Light blue icon color */
    transition: color 0.3s;
}

.formItem:hover .icon {
    color: #fbbf24; /* Icon changes to gold on hover */
}

/* Active form item */
.active {
    background: #64748b; /* Different background for active state */
    color: #ffffff;
}

.active .icon {
    color: #fbbf24; /* Gold icon for active state */
}
