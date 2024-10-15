/* Main container for the whole dashboard */
.dashboardContainer {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 20px;
    height: 100%;
    padding: 20px;
    background: linear-gradient(135deg, #1e293b, #334155);
    margin: 0 auto;
}

/* Styling each card (similar to widgets in the screenshot) */
.card {
    background-color: #fff;
    border: 1px solid #d1d5db;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
    padding: 10px;
    color: #374151;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: height 0.3s ease, padding 0.3s ease;
    height: auto;
    max-height: 200px; /* When collapsed */
    overflow: hidden;
}

.card.expanded {
    max-height: none;
    padding-bottom: 20px;
}

/* Card Header */
.card h3 {
    font-size: 1rem;
    color: #1f2937;
    margin-bottom: 10px;
}

/* Form Data Section */
.formDataSection {
    margin-top: 20px;
}

.formDataTitle {
    font-size: 0.8rem;
    font-weight: 600;
    color: #2563eb;
}

.formDataContent {
    font-size: 0.7rem;
    color: #4b5563;
}

/* Icon styling */
.cardIcon {
    margin-right: 8px;
    color: #6b7280;
}

/* View All button styling */
.viewAllButton {
    background-color: #2563eb;
    color: #fff;
    border: none;
    padding: 5px 10px;
    font-size: 0.8rem;
    cursor: pointer;
    margin-top: 10px;
    border-radius: 5px;
    transition: background-color 0.3s ease;
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
