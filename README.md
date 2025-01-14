/* Copy Button */
.copy-btn:hover {
  background-color: var(--primary-color);
  color: white;
  box-shadow: 0px 4px 10px rgba(120, 92, 229, 0.3);
}

/* "Copied" State */
.copy-btn.copied {
  background-color: #4caf50; /* Success Green */
  color: white;
}

/* Trace Details Section */
.trace-details-section h4 {
  font-size: 1rem;
  margin-bottom: var(--space-sm);
  display: flex;
  align-items: center;
  gap: 8px;
  color: var(--text-primary);
  border-bottom: 1px solid var(--border-color);
  padding-bottom: var(--space-xs);
}

.trace-details-section .json-viewer-container {
  margin-top: var(--space-md);
  background-color: var(--background-dark);
  padding: var(--space-md);
  border-radius: 8px;
  box-shadow: var(--shadow-subtle);
  overflow: auto;
}

/* No Trace Data Message */
.no-trace-data {
  text-align: center;
  padding: var(--space-md);
  font-size: 0.9rem;
  color: var(--text-secondary);
  background-color: var(--background-light);
  border: 1px dashed var(--border-color);
  border-radius: 8px;
}

/* Add Scrollbar Customization */
.trace-content::-webkit-scrollbar {
  width: 8px;
}

.trace-content::-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 4px;
}

.trace-content::-webkit-scrollbar-thumb:hover {
  background-color: var(--secondary-color);
}

/* JSON Viewer Styling */
.json-key {
  font-family: var(--font-family-mono);
  color: var(--primary-color);
  margin-right: 4px;
}

.json-null {
  color: #ff6b6b; /* Red for null values */
}

.json-string {
  color: #499dfd; /* Blue for strings */
}

.json-number {
  color: #fa8231; /* Orange for numbers */
}

.json-boolean {
  color: #20c997; /* Green for booleans */
}

.json-array, .json-object {
  padding-left: var(--space-sm);
}

/* Interactive Animations */
.trace-step-header:hover .expand-icon {
  transform: scale(1.1);
}

.trace-step-header:hover {
  background-color: rgba(120, 92, 229, 0.05);
  transition: background-color 0.3s ease-in-out;
}

/* Responsive Design */
@media (max-width: 768px) {
  .trace-step-number {
    font-size: 0.9rem;
  }

  .trace-agent-name {
    font-size: 0.7rem;
  }

  .trace-step-details {
    padding: var(--space-sm);
  }

  .trace-details-section h4 {
    font-size: 0.9rem;
  }
}
