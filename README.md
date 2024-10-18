/* Login.module.css */

.container {
  display: flex;
  height: 100vh;
  background: rgb();
}

.leftSide {
  display: none;
  justify-content: center;
  align-items: center;
  width: 50%;
  background-image: url('../assets/LoginImg.jpg');
  background-size: cover;
  background-position: center;
}

.overlayText {
  color: white;
  font-size: 2rem;
  font-weight: bold;
  padding: 1.5rem;
  text-align: center;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 0.5rem;
}

.rightSide {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 50%;
  padding: 2rem;
  background-color: white;
  /*border-radius: 0.5rem;*/
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
}

.formContainer {
  max-width: 400px;
  margin: 0 auto;
}

.title {
  font-size: 2rem;
  font-weight: bold;
  text-align: center;
  color: #4f46e5;
  margin-bottom: 1.5rem;
}

.label {
  font-weight: bold;
  color: #4a5568;
  margin-bottom: 0.5rem;
}

.input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #e2e8f0;
  border-radius: 0.375rem;
  margin-bottom: 1.5rem;
}

.button {
  width: 100%;
  padding: 0.75rem;
  background-color: #4f46e5;
  color: white;
  font-weight: bold;
  border-radius: 0.375rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.button:hover {
  background-color: #4338ca;
}

.toggleText {
  margin-top: 1rem;
  text-align: center;
  color: #4f46e5;
  font-weight: bold;
  cursor: pointer;
}

@media (min-width: 768px) {
  .leftSide {
    display: flex;
  }
}
