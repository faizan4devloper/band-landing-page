/* Container for the form */
.claim-form {
  background: linear-gradient(145deg, #f3f3f3, #ffffff);
  padding: 40px;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  max-width: 600px;
  margin: 20px auto;
  font-family: "Roboto", sans-serif;
  transition: transform 0.3s ease;
}

.claim-form:hover {
  transform: translateY(-5px);
}

/* Form heading */
.claim-form h2 {
  font-size: 2.2rem;
  color: #333;
  margin-bottom: 25px;
  text-align: center;
  font-weight: 700;
  letter-spacing: 1.5px;
}

/* Input fields styling */
.form-group {
  margin-bottom: 25px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.form-group label {
  font-size: 1.3rem;
  color: #666;
  margin-bottom: 5px;
  font-weight: 500;
}

.form-group input[type="text"],
.form-group select {
  padding: 14px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #f9f9f9;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  outline: none;
}

/* Focus effect for input fields */
.form-group input[type="text"]:focus,
.form-group select:focus {
  border-color: #1f77f6;
  box-shadow: 0 0 10px rgba(31, 119, 246, 0.3);
}

/* Radio buttons styling */
.radio-btn {
  display: flex;
  justify-content: center;
  gap: 30px;
  margin-top: 15px;
}

.radio-btn label {
  font-size: 1.2rem;
  color: #333;
  display: flex;
  align-items: center;
  cursor: pointer;
}

.radio-btn input[type="radio"] {
  margin-right: 10px;
  accent-color: #1f77f6;
}

/* Custom styling for Select dropdown */
.Select__control {
  border-radius: 10px;
  border: 1px solid #ccc;
  padding: 12px;
}

.Select__option--is-focused {
  background-color: #1f77f6 !important;
  color: #fff !important;
}

.Select__single-value {
  font-size: 1.1rem;
}

/* PDF document container styling */
.pdf-container {
  margin-top: 25px;
  background-color: #f1f1f1;
  padding: 20px;
  border: 2px dashed #1f77f6;
  border-radius: 12px;
  text-align: center;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

.pdf-container p {
  font-size: 1rem;
  color: #666;
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
  border-radius: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  margin-top: 30px;
  text-transform: uppercase;
  letter-spacing: 1.2px;
  font-weight: 600;
}

.cta-button:hover {
  background-color: #145bb5;
  box-shadow: 0 10px 20px rgba(20, 91, 181, 0.4);
}

/* Responsive layout adjustments */
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
