.claimIdDisplay {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f2f5fc; /* Subtle background */
  padding: 10px 20px;
  border-radius: 8px;
  font-size: 18px;
  font-weight: bold;
  color: #2c3e50;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Subtle shadow */
}


.previewSection {
  background: #ffffff;
  padding: 15px;
  border: 1px solid #e3e6f0;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.previewSection:hover {
  transform: scale(1.02);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.documentPreviewIframe {
  width: 100%;
  height: 670px;
  border: none;
  border-radius: 8px;
  overflow: hidden;
  background: #f8f9fc; /* Neutral background while loading */
}



.extractContentSection {
  background: #ffffff;
  padding: 15px;
  border: 1px solid #e3e6f0;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.extractContentSection:hover {
  transform: scale(1.02);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.readableContent {
  background: #f8f8f8;
  padding: 20px;
  border-radius: 8px;
  line-height: 1.8;
  font-size: 16px;
  color: #2c3e50;
}

.readableContent h4 {
  margin-bottom: 10px;
  font-size: 1.2rem;
  color: #34495e;
  border-bottom: 2px solid #7ca2e1; /* Accent border */
  display: inline-block;
  padding-bottom: 5px;
}

.readableContent p {
  margin: 10px 0;
}



.reloadButton, .verifyButton {
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  padding: 12px 24px;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px; /* Space between icon and text */
}

.reloadButton:hover, .verifyButton:hover {
  transform: translateY(-2px);
  background: linear-gradient(90deg, #0056b3, #0084ff, #0056b3);
}

.reloadButton:active, .verifyButton:active {
  transform: scale(0.98);
}



const renderReadableContent = (data) => {
  if (!data) return <p>No data available</p>;

  const parsedData = JSON.parse(data);

  return (
    <div className={styles.readableContent}>
      {Object.entries(parsedData).map(([key, value]) => (
        <div key={key} className={styles.dataSection}>
          <h4>{key.replace(/_/g, ' ')}</h4>
          <ul>
            {Object.entries(value).map(([subKey, subValue]) => (
              <li key={subKey}>
                <strong>{subKey.replace(/_/g, ' ')}:</strong> {subValue || 'N/A'}
              </li>
            ))}
          </ul>
        </div>
      ))}
    </div>
  );
};



.dataSection {
  margin-bottom: 20px;
  padding: 10px;
  background: #f9fafc;
  border: 1px solid #e3e6f0;
  border-radius: 8px;
}

.dataSection h4 {
  font-size: 1.1rem;
  color: #2c3e50;
  margin-bottom: 10px;
}

.dataSection ul {
  list-style: none;
  padding: 0;
}

.dataSection li {
  margin-bottom: 8px;
  color: #555;
}



@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.mainContent, .previewSection, .extractContentSection {
  animation: fadeIn 0.5s ease-in-out;
}
