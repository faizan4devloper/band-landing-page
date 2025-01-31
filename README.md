.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-top: 20px;
  background-color: #f4f6f9;
  padding: 15px;
  border-radius: 12px;
}

.paginationButton {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 8px 12px;
  background-color: #4a90e2;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.3s;
}

.paginationButton:hover {
  background-color: #0056b3;
}

.paginationButton.disabled {
  background-color: #a0a0a0;
  cursor: not-allowed;
}

.paginationButton.active {
  background-color: #4a90e2;
  border: 2px solid #0056b3;
}

.paginationNumbers {
  display: flex;
  gap: 5px;
}

.paginationIcon {
  margin-left: 5px;
  font-size: 16px;
}
