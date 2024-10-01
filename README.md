/* Container styling */
.sidebar {
    background-color: #3b4e73; /* Mid-tone blue to match the app gradient */
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    height: 100%; /* Full height as defined by parent */
    display: flex;
    flex-direction: column;
    gap: 15px; /* Space between items */
    overflow-y: auto; /* Allow scrolling if necessary */
}

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
    background: #4f6a94; /* Darker item background */
    border-radius: 8px;
    color: #e2e8f0; /* Light text color */
    font-size: 1.1rem;
    cursor: pointer;
    transition: all 0.3s ease;
}

.formItem:hover {
    background: #617da8; /* Lighter on hover */
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
    background: #7588b2; /* Slightly lighter background for active state */
    color: #ffffff;
}

.active .icon {
    color: #fbbf24; /* Gold icon for active state */
}
