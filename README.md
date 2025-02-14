.claimClassificationContainer {
  max-width: 700px;
  margin: 0 auto;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  background-color: #ffffff;
  padding: 20px;
  transition: all 0.3s ease;
}

h4 {
  text-align: center;
  color: #2d3748;
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 15px;
}

.classificationTable {
  width: 100%;
  border-collapse: collapse;
  border-radius: 8px;
  overflow: hidden;
}

.classificationTable th, .classificationTable td {
  border: 1px solid #e1e8f0;
  padding: 12px;
  text-align: center;
  font-size: 14px;
}

.classificationTable th {
  background-color: #f7fafc;
  font-weight: 700;
  color: #2d3748;
}

.classificationTable td {
  color: #4a5568;
}

/* Classification Tag Styles */
.classificationTag {
  display: inline-block;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.cancerTag {
  background-color: #ffebee;
  color: #d32f2f;
}

.heartTag {
  background-color: #e8f5e9;
  color: #2e7d32;
}

.defaultTag {
  background-color: #f5f5f5;
  color: #616161;
}

/* Validity Tag Styles */
.validTag {
  display: inline-block;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.validYes {
  background-color: #e8f5e9;
  color: #2e7d32;
}

.validNo {
  background-color: #ffebee;
  color: #d32f2f;
}

/* Hover Effect */
.classificationTable tr:hover {
  background-color: #f1f5f9;
}

/* Responsive Design */
@media (max-width: 600px) {
  .claimClassificationContainer {
    max-width: 95%;
  }

  .classificationTable th, .classificationTable td {
    padding: 8px;
  }
}
