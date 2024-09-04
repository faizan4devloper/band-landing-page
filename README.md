/* Styles for the pop-up within the card */
.cardPopup {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  text-align: center;
  width: 80%; /* Adjust width to fit within the card */
  z-index: 10; /* Ensure the pop-up appears above other content */
  animation: fadeIn 0.3s ease-in-out;
}

/* Close button inside the pop-up */
.closeButton {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
  margin-top: 10px;
}

.closeButton:hover {
  background-color: #0056b3;
}

/* Fade-in animation for the pop-up */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translate(-50%, -45%);
  }
  to {
    opacity: 1;
    transform: translate(-50%, -50%);
  }
}
