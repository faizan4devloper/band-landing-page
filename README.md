.mainContent {
  display: flex;
  gap: 20px;
  padding: 20px;
}

.previewSection,
.extractContentSection {
  flex: 1;
  padding: 10px;
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.documentList {
  list-style: none;
  padding: 0;
}

.documentList li {
  margin-bottom: 10px;
}

.previewButton {
  background-color: #007bff;
  color: white;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.previewButton:hover {
  background-color: #0056b3;
}

.extractedData {
  background: #f8f8f8;
  padding: 10px;
  border-radius: 5px;
  overflow-x: auto;
}

.reloadButton {
  margin-top: 10px;
  background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.reloadButton:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.readableContent {
  background: #f8f8f8;
  padding: 15px;
  border-radius: 8px;
  line-height: 1.6;
}

.readableContent h4 {
  color: #333;
  margin-bottom: 10px;
  text-decoration: underline;
}

.readableContent p {
  margin: 5px 0;
  color: #555;
}

.readableContent strong {
  color: #000;
}

h5 {
  margin-top: 15px;
  font-size: 1.1rem;
  color: #444;
}
