.formContainer {
  padding: 10px;
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
}

.demoHead {
  color: #5f1ec1;
  margin-bottom: 10px;
  text-align: center;
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
  padding: 4px;
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

/* Styling for Select */
.select {
  width: 92%;
  font-size: 12px; /* Decrease font size for the Select */
}

.select__control {
  border: 1px solid #5f1ec1 !important; /* Ensure control border is applied */
  border-radius: 4px !important;
  font-family: "Poppins", sans-serif !important;
  font-size: 12px !important; /* Ensure control text is 12px */
  color: #333 !important;
}

.select__control--is-focused {
  border-color: #5f1ec1 !important;
  box-shadow: 0 0 0 1px #5f1ec1 !important;
}

.select__menu {
  font-size: 12px !important; /* Ensure menu text is 12px */
}

.select__option {
  padding: 8px 8px !important;
  font-size: 12px !important; /* Ensure option text is 12px */
}

.select__option--is-focused {
  background-color: #5f1ec1 !important; /* Change background color on focus */
  color: white !important; /* Change text color on focus */
}

.select__option--is-selected {
  background-color: #5f1ec1 !important; /* Change background color on selection */
  color: white !important; /* Change text color on selection */
}

.submitButton {
  background-color: #5f1ec1;
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
  top: 50px;
  right: 25px;
  background-color: transparent;
  border: none;
  cursor: pointer;
  font-size: 24px;
  color: #aaa;
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
  width: 10px;
}

.formContainer::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 8px;
}

.formContainer::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 8px;
}

.formContainer::-webkit-scrollbar-thumb:hover {
  background: #555;
}
