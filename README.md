/* Container for the page layout */
.container {
  display: flex;
  height: 100vh;
}

/* Left side background with overlay */
.leftSide {
  display: none;
  justify-content: center;
  align-items: center;
  width: 50%;
  position: relative;
  background-size: cover;
  background-position: center;
  overflow: hidden; /* Hide any overflow from animations */
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1;
}

/* Fade-in and slide-in effect for headline text */
.overlayText {
  position: relative;
  z-index: 2;
  color: white;
  font-size: 2.5rem;
  font-weight: bold;
  padding: 2rem;
  text-align: center;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 0.75rem;
  opacity: 0; /* Initially hidden */
  transform: translateY(-20px); /* Start a little above */
  animation: fadeInSlide 1s ease-out forwards;
}

/* Fade-in for description */
.leftSide p {
  position: relative;
  z-index: 2;
  color: #f0f0f0;
  text-align: center;
  font-size: 1.2rem;
  padding: 1.5rem;
  margin: 0;
  opacity: 0; /* Initially hidden */
  transform: translateY(20px); /* Start a little below */
  animation: fadeInSlide 1.2s ease-out forwards;
  animation-delay: 0.3s; /* Delay the description animation slightly */
}

/* Right side with form */
.rightSide {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 50%;
  padding: 3rem;
  background-color: transparent;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  border-radius: 1rem;
}

.formContainer {
  max-width: 400px;
  margin: 0 auto;
}

.title {
  font-size: 2.5rem;
  font-weight: bold;
  text-align: center;
  color: #4f46e5;
  margin-bottom: 2rem;
}

/* Input fields and button styles */
.inputGroup {
  display: flex;
  align-items: center;
  background-color: #f3f4f6;
  padding: 0.75rem;
  border-radius: 0.375rem;
  margin-bottom: 1.5rem;
  border: 1px solid #e2e8f0;
}

.icon {
  margin-right: 0.75rem;
  color: #4f46e5;
  font-size: 1.2rem;
}

.input {
  width: 100%;
  padding: 0.75rem;
  border: none;
  background: transparent;
  outline: none;
  font-size: 1rem;
}

/* Button with hover effect */
.button {
  width: 100%;
  padding: 0.85rem;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: white;
  font-weight: bold;
  border: none;
  border-radius: 0.375rem;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.button:hover {
  background-color: #4338ca;
  transform: translateY(-2px);
}

/* Animation keyframes for fade-in and slide-up */
@keyframes fadeInSlide {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (min-width: 768px) {
  .leftSide {
    display: flex;
  }
}
