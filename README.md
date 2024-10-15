/* Main container for the whole dashboard */
.dashboardContainer {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 20px;
    padding: 20px;
    background: linear-gradient(135deg, #1e293b, #334155);
    margin: 0 auto;
    width: 100%;
}

/* Styling each card */
.card {
    background-color: #fff;
    border: 1px solid #d1d5db;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
    padding: 20px;
    color: #374151;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    overflow: hidden;
    max-height: 200px;  /* Default card height when collapsed */
    transition: max-height 0.5s ease-in-out, padding-bottom 0.3s ease;
    position: relative;
}

/* Expanded card */
.card.expanded {
    max-height: 1000px;  /* Large enough to show full content */
}

/* Card Header */
.card h3 {
    font-size: 1rem;
    color: #1f2937;
    margin-bottom: 10px;
}

/* Form Data Section */
.formDataSection {
    margin-top: 10px;
}

.formDataTitle {
    font-size: 0.9rem;
    font-weight: 600;
    color: #2563eb;
    display: flex;
    align-items: center;
}

.formDataContent {
    font-size: 0.8rem;
    color: #4b5563;
    margin-left: 25px;
}

/* Icon styling */
.cardIcon {
    margin-right: 8px;
    color: #6b7280;
}

/* "View All" button */
.viewAllButton {
    background-color: #2563eb;
    color: #fff;
    border: none;
    padding: 8px 12px;
    font-size: 0.85rem;
    cursor: pointer;
    margin-top: 10px;
    border-radius: 5px;
    transition: background-color 0.3s ease;
    align-self: flex-end;
}

.viewAllButton:hover {
    background-color: #1d4ed8;
}

/* Responsive Layout */
@media (max-width: 768px) {
    .dashboardContainer {
        grid-template-columns: 1fr;
    }
}
