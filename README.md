.formContainer {
  padding: 20px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 450px;
  margin: 0 auto;
  animation: slideIn 0.5s ease-out;
  max-height: 80vh; /* Set a max-height for the form container */
  overflow-y: auto; /* Enable vertical scrolling */
  z-index: 1000; /* Ensure the form container has a high z-index */
}

.demoHead {
  color: #5f1ec1;
  margin-bottom: 10px;
  text-align: center;
  margin-top: -25px;
  font-size: 18px;
}

.formGroup {
  margin-bottom: 10px;
  position: relative;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  color: #000;
  font-size: 12px;
  transition: color 0.3s;
}

input::placeholder, textarea::placeholder {
  color: #999999; /* Light gray */
  opacity: 1;
}

input, textarea {
  width: 90%;
  padding: 0px;
  border: none;
  border-bottom: 1px solid #ccc; /* Only bottom border */
  border-radius: 0;
  font-size: 12px;
  transition: border-color 0.3s;
  font-family: "Poppins", sans-serif; /* Apply Google Font */
}

input:focus, textarea:focus {
  border-color: #5f1ec1;
  outline: none;
}

.submitButton {
  background-color: #5f1ec1;
  margin-top: 0px;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  margin-top: 10px;
  transition: background-color 0.3s;
  margin-left: 30px;
  width: 78%; /* Make the button take the full width */
}

.closeButton {
  position: absolute;
  top: 25px;
  right: 25px;
  background-color: transparent;
  border: none;
  cursor: pointer;
  font-size: 24px;
  color: #aaa;
  z-index: 1001; /* Slightly higher than the form container */
}

.closeButton:hover {
  color: #555;
}

.successMessage {
  text-align: center;
  color: #5f1ec1;
  font-size: 16px;
}

/* Custom scrollbar styling */
.formContainer::-webkit-scrollbar {
  width: 6px;
}

.formContainer::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 8px;
}

.formContainer::-webkit-scrollbar-thumb {
  background: #5f1ec1;
  border-radius: 8px;
}

.formContainer::-webkit-scrollbar-thumb:hover {
  background: #555;
}

/* Carousel styles */
.carouselContainer .carousel .control-dots {
  background-color: rgba(95, 30, 193, 1) !important;
  z-index: 10 !important; /* Lower the z-index of the carousel indicators */
}

.carouselContainer .carousel .control-dots .dot.selected {
  background-color: #6f36cd !important;
}






.carouselContainer .carousel .control-dots {
  background-color: rgba(95, 30, 193, 1) !important;
  z-index: 10 !important; /* Lower the z-index of the carousel indicators */
}

.carouselContainer .carousel .control-dots .dot.selected {
  background-color: #6f36cd !important;
}
