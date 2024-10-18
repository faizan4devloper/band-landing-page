/* LoginPage.module.css */

.container {
  display: flex;
  height: 100vh;
  width: 100vw;
  justify-content: center;
  align-items: center;
  position: relative;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  /* This ensures the image covers both sides */
}

/* Overlay for contrast */
.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5); /* Dark overlay for contrast */
  z-index: 1;
}

/* Form container positioned over the background */
.formContent {
  position: relative;
  z-index: 2;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.formContainer {
  width: 100%;
  max-width: 400px;
  padding: 2rem;
  background-color: rgba(255, 255, 255, 0.9); /* Slightly transparent background */
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  border-radius: 1rem;
}

.title {
  font-size: 2.5rem;
  font-weight: bold;
  text-align: center;
  color: #4f46e5;
  margin-bottom: 2rem;
}

.inputGroup {
  display: flex;
  align-items: center;
  background-color: #f3f4f6;
  padding: 0.75rem;
  border-radius: 0.375rem;
  margin-bottom: 1.5rem;
  border: 1px solid #e2e8f0;
}

.input {
  width: 100%;
  padding: 0.75rem;
  border: none;
  background: transparent;
  outline: none;
  font-size: 1rem;
}

.button {
  width: 100%;
  padding: 0.85rem;
  background-color: #4f46e5;
  color: white;
  font-weight: bold;
  border-radius: 0.375rem;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.button:hover {
  background-color: #4338ca;
  transform: translateY(-2px);
}

.toggleText {
  margin-top: 1.5rem;
  text-align: center;
  color: #4f46e5;
  font-weight: bold;
  cursor: pointer;
}

@media (min-width: 768px) {
  .container {
    flex-direction: row;
  }
}
