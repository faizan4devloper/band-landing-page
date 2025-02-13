/* NewClaimPage.module.css - Updated */
.container {
  display: flex;
  height: 100vh;
  background: #f8fafc;
  position: relative;
}

.sidebar {
  flex: 0 0 320px;
  background: linear-gradient(195deg, #ffffff 0%, #f8fafc 100%);
  border-right: 1px solid #e2e8f0;
  box-shadow: 4px 0 6px -1px rgba(0, 0, 0, 0.05);
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 100;
}

.mainContent {
  flex: 1;
  padding: 2rem;
  overflow-y: auto;
  background: #f8fafc;
}

.toggleButton {
  position: fixed;
  left: 1rem;
  top: 1rem;
  background: #3b82f6;
  padding: 0.75rem;
  border-radius: 0.5rem;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  transition: all 0.2s ease;
}

/* MainContent.module.css - Enhanced */
.mainContentWrapper {
  max-width: 1400px;
  margin: 0 auto;
}

.claimIdDisplay {
  background: #ffffff;
  padding: 1rem 1.5rem;
  border-radius: 0.75rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  margin-bottom: 2rem;
}

.claimIdDisplay h3 {
  color: #1e293b;
  font-size: 1.25rem;
  font-weight: 600;
  margin: 0;
}

.mainContentGrid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
}

/* Left Column Styles */
.leftColumn {
  display: grid;
  gap: 1.5rem;
  grid-template-rows: auto 1fr;
}

.classificationCard {
  background: #ffffff;
  border-radius: 1rem;
  padding: 1.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
}

.documentPreviewContainer {
  background: #ffffff;
  border-radius: 1rem;
  padding: 1.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  min-height: 500px;
}

/* Right Column Styles */
.rightColumn {
  display: grid;
  gap: 1.5rem;
  grid-template-rows: auto 1fr auto;
}

.percentageCard {
  background: #ffffff;
  border-radius: 1rem;
  padding: 2rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  display: flex;
  align-items: center;
  justify-content: center;
  height: 180px;
}

.extractedContentCard {
  background: #ffffff;
  border-radius: 1rem;
  padding: 1.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  overflow: hidden;
}

/* Progress Circle */
.progressCircle {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: conic-gradient(#3b82f6 calc(var(--progress) * 3.6deg), #e2e8f0 0deg);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.progressInner {
  width: 90px;
  height: 90px;
  background: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  color: #1e293b;
}

/* Classification Table */
.classificationTable {
  width: 100%;
  border-collapse: collapse;
}

.classificationTable th {
  text-align: left;
  padding: 0.75rem 1rem;
  background: #f8fafc;
  color: #64748b;
  font-weight: 500;
  border-bottom: 2px solid #e2e8f0;
}

.classificationTable td {
  padding: 1rem;
  border-bottom: 1px solid #e2e8f0;
  color: #1e293b;
}

/* Verify Button */
.verifyButton {
  background: #3b82f6;
  color: white;
  padding: 1rem 2rem;
  border-radius: 0.75rem;
  font-weight: 600;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  justify-content: center;
}

.verifyButton:hover {
  background: #2563eb;
  transform: translateY(-1px);
}

@media (max-width: 1024px) {
  .mainContentGrid {
    grid-template-columns: 1fr;
  }
  
  .documentPreviewContainer {
    min-height: 400px;
  }
}



<div className={styles.percentageCard}>
  <div 
    className={styles.progressCircle} 
    style={{ '--progress': percentage }}
  >
    <div className={styles.progressInner}>
      {percentage}%
    </div>
  </div>
</div>
