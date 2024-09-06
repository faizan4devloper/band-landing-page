/* Container for the form */
.claim-form {
  background-color: #ffffff;
  padding: 40px;
  border-radius: 12px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  margin: 20px auto;
  font-family: "Roboto", sans-serif;
}

/* Form heading */
.claim-form h2 {
  font-size: 2rem;
  color: #333;
  margin-bottom: 20px;
  text-align: center;
  font-weight: 600;
}

/* Input fields styling */
.form-group {
  margin-bottom: 25px;
  display: flex;
  flex-direction: column;
}

.form-group label {
  font-size: 1.2rem;
  color: #555;
  margin-bottom: 10px;
}

.form-group input[type="text"],
.form-group select {
  padding: 12px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  transition: border-color 0.3s ease;
  outline: none;
}

/* Focus effect for input fields */
.form-group input[type="text"]:focus,
.form-group select:focus {
  border-color: #1f77f6;
}

/* Radio buttons styling */
.radio-btn {
  display: flex;
  justify-content: center;
  gap: 25px;
}

.radio-btn label {
  font-size: 1rem;
  color: #555;
}

.radio-btn input[type="radio"] {
  margin-right: 10px;
}

/* PDF document container styling */
.pdf-container {
  margin-top: 20px;
  background-color: #f9f9f9;
  padding: 20px;
  border: 2px dashed #ccc;
  border-radius: 8px;
  text-align: center;
}

/* Submit button styling */
.cta-button {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 1.1rem;
  background-color: #1f77f6;
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.cta-button:hover {
  background-color: #1a60c1;
}

/* Form group styles for a responsive layout */
@media screen and (max-width: 768px) {
  .claim-form {
    padding: 20px;
    max-width: 90%;
  }

  .form-group input[type="text"],
  .form-group select {
    padding: 10px;
  }

  .cta-button {
    padding: 12px;
  }
}
