 <button
    className={styles.verifyButton}
    onClick={() => console.log("Verify action triggered for Extract Content")}
  >
    Verify
  </button>



  .verifyButton {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #007bff; /* Blue color for primary button */
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.verifyButton:hover {
  background-color: #0056b3; /* Darker blue for hover */
  transform: scale(1.05); /* Slightly enlarge on hover */
}

.verifyButton:active {
  transform: scale(1); /* Reset scale on click */
}

.verifyButton:disabled {
  background-color: #ccc; /* Disabled state color */
  cursor: not-allowed;
}
