/* RequestDemoForm.module.css */

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

input::placeholder {
  color: #999999; /* Light gray */
  opacity: 1;
}

input {
  width: 90%;
  padding: 4px;
  border: none;
  border-bottom: 1px solid #ccc; /* Only bottom border */
  border-radius: 0;
  font-size: 12px;
  transition: border-color 0.3s;
  font-family: "Poppins", sans-serif; /* Apply Google Font */
}

input:focus {
  border-color: #5f1ec1;
  outline: none;
}

/* Styling for Select */
.select {
  width: 90%;
  font-size: 12px; /* Decrease font size for the Select */
  margin-left: 10px; /* Adjust margin as needed */
}

.select__control {
  border: 1px solid #ccc;
  border-radius: 4px;
  font-family: "Poppins", sans-serif;
  font-size: 12px; /* Ensure control text is 12px */
  color: #333;
}

.select__control--is-focused {
  border-color: #5f1ec1;
  box-shadow: 0 0 0 1px #5f1ec1;
}

.select__menu {
  font-size: 12px; /* Ensure menu text is 12px */
}

.select__option {
  padding: 8px 8px;
  font-size: 12px; /* Ensure option text is 12px */
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
  top: 20px;
  right: 10px;
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
