/* ClaimClassification.module.css */
.container {
  background: #ffffff;
  border-radius: 16px;
  padding: 1.5rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid #f0f4f8;
}

.container:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 24px rgba(0, 0, 0, 0.1);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.25rem;
  gap: 1rem;
}

h3 {
  color: #2d3748;
  font-size: 1.1rem;
  font-weight: 600;
  margin: 0;
  line-height: 1.4;
}

.badge {
  padding: 0.35rem 1rem;
  border-radius: 20px;
  font-size: 0.85rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  white-space: nowrap;
}

.badge[data-type="cancer"] {
  background: #fff5f5;
  color: #c53030;
  border: 1px solid #fed7d7;
}

.badge[data-type="heart"] {
  background: #f0fff4;
  color: #2f855a;
  border: 1px solid #c6f6d5;
}

.badge[data-type="unknown"] {
  background: #edf2f7;
  color: #4a5568;
  border: 1px solid #e2e8f0;
}

.content {
  display: grid;
  gap: 1.25rem;
}

.infoRow {
  display: grid;
  grid-template-columns: 120px 1fr;
  align-items: baseline;
  gap: 0.75rem;
}

label {
  color: #718096;
  font-size: 0.9rem;
  font-weight: 500;
}

p {
  margin: 0;
  color: #2d3748;
  font-weight: 500;
  word-break: break-word;
  line-height: 1.5;
}

.statusIndicator {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-top: 0.5rem;
  padding: 0.75rem;
  background: #f8fafc;
  border-radius: 8px;
}

.indicatorDot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #48bb78;
  box-shadow: 0 2px 4px rgba(72, 187, 120, 0.2);
}

.indicatorDot[data-valid="false"] {
  background: #f56565;
  box-shadow: 0 2px 4px rgba(245, 101, 101, 0.2);
}

@media (max-width: 768px) {
  .container {
    padding: 1.25rem;
  }

  .infoRow {
    grid-template-columns: 1fr;
    gap: 0.5rem;
  }

  .badge {
    font-size: 0.75rem;
    padding: 0.3rem 0.8rem;
  }

  .statusIndicator {
    font-size: 0.9rem;
  }
}

@media (max-width: 480px) {
  .header {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.75rem;
  }
  
  .badge {
    align-self: flex-start;
  }
}
