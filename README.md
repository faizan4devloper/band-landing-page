/* MainContent.module.css */
.mainContentGrid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); /* More responsive */
  gap: 1rem;
  padding: 1rem;
  height: auto; /* Remove fixed height */
}


.cardContainer {
  background: white;
  border-radius: 8px;
  padding: 1.25rem;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  margin-bottom: 1rem;
}


.claimClassificationContainer {
  max-height: 400px;
  overflow: auto;
}

/* Add to your table CSS */
.compact-table {
  font-size: 0.875rem;
  border-collapse: collapse;
}
.compact-table td, .compact-table th {
  padding: 0.5rem;
  border-bottom: 1px solid #eee;
}


.percentageSection {
  height: auto; /* Remove fixed height */
  padding: 1rem;
}


.documentPreviewContainer {
  max-height: 500px;
  min-height: 300px;
  position: relative;
}

/* Update these styles */
.claimIdDisplay {
  margin: 1rem 0;
  padding: 0.5rem 1rem;
  background: #f8f9fa;
  border-radius: 6px;
}

.verifyButton {
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  border-radius: 6px;
}

@media (max-width: 768px) {
  .mainContentGrid {
    grid-template-columns: 1fr;
  }
  
  .leftColumn, .rightColumn {
    gap: 0.5rem;
  }
  
  .cardContainer {
    padding: 1rem;
  }
}
