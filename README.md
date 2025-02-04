/* ClaimClassification.module.css */

.claimClassificationContainer {
  width: 100%;
  max-width: 800px;
  margin: 20px auto;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  overflow: hidden;
  background-color: #ffffff;
  transition: all 0.3s ease;
  animation: fadeIn 0.5s ease-out;
}

.classificationTable {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  font-family: 'Inter', 'Arial', sans-serif;
  background: white;
}

/* Enhanced Header Styles */
.classificationTable thead {
  background: linear-gradient(135deg, #6366f1 0%, #3b82f6 100%);
}

.classificationTable thead tr {
  height: 50px;
}

.classificationTable thead th {
  padding: 16px 24px;
  color: white;
  font-weight: 600;
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: 0.75px;
  border-bottom: none;
}

/* Improved Body Styles */
.classificationTable tbody tr {
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
}

.classificationTable tbody tr:not(:last-child) td {
  border-bottom: 1px solid #f1f5f9;
}

.classificationTable tbody td {
  padding: 18px 24px;
  color: #334155;
  font-weight: 500;
  font-size: 0.95rem;
}

/* Hover Effects */
.classificationTable tbody tr:hover {
  background-color: #f8fafc;
  transform: translateX(4px);
  box-shadow: -4px 0 0 0 #3b82f6;
}

/* Tag Base Styles */
.classificationTag {
  display: inline-flex;
  align-items: center;
  padding: 6px 14px;
  border-radius: 20px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  font-size: 0.8rem;
  border: 1.5px solid transparent;
  transition: all 0.2s ease;
}

/* Classification Tags */
.cancerTag {
  background-color: #fef2f2;
  color: #dc2626;
  border-color: #fecaca;
}

.heartTag {
  background-color: #f0fdf4;
  color: #16a34a;
  border-color: #bbf7d0;
}

.defaultTag {
  background-color: #f8fafc;
  color: #64748b;
  border-color: #e2e8f0;
}

/* Validity Status Styles */
.validYes, .validNo {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 14px;
  border-radius: 20px;
  font-weight: 600;
  text-transform: uppercase;
  font-size: 0.8rem;
  letter-spacing: 0.5px;
}

.validYes {
  background-color: #f0fdf4;
  color: #16a34a;
  border: 1.5px solid #bbf7d0;
}

.validYes::before {
  content: '✓';
  display: inline-block;
  font-weight: 700;
}

.validNo {
  background-color: #fef2f2;
  color: #dc2626;
  border: 1.5px solid #fecaca;
}

.validNo::before {
  content: '✗';
  display: inline-block;
  font-weight: 700;
}

/* Responsive Design */
@media (max-width: 600px) {
  .claimClassificationContainer {
    margin: 12px;
    width: calc(100% - 24px);
    border-radius: 8px;
  }

  .classificationTable thead th,
  .classificationTable tbody td {
    padding: 12px 16px;
    font-size: 0.85rem;
  }

  .classificationTag,
  .validYes,
  .validNo {
    font-size: 0.75rem;
    padding: 4px 10px;
  }
}

/* Animations */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}




<td className={`${styles.classificationTag} ${
  classification.toLowerCase() === 'cancer' 
    ? styles.cancerTag 
    : classification.toLowerCase() === 'heart' 
      ? styles.heartTag 
      : styles.defaultTag
}`}>
  {classification}
</td>
