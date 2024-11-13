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

/* Animated and styled headline text */
.overlayText {
  position: relative;
  z-index: 2;
  color: white;
  font-size: 2.5rem;
  font-weight: bold;
  padding: 4rem 2rem;
  text-align: center;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 0.75rem;
  opacity: 0; /* Initially hidden */
  transform: translateY(-20px); /* Start a little above */
  animation: fadeInSlide 1s ease-out forwards;
}

/* Gradient animation on text */
.overlayText span {
  background: linear-gradient(90deg, #7abef7, #dfdfdf, #572ac2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
  animation: gradientMove 4s ease-in-out infinite;
}

@keyframes gradientMove {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

/* Enhanced text animation for description */
.leftSide p {
  position: relative;
  z-index: 2;
  color: #f0f0f0;
  text-align: center;
  font-size: 1rem;
  font-weight: normal;
  padding: 1.5rem;
  margin: 0;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInSlide 1.2s ease-out forwards;
  animation-delay: 0.3s;
}

/* Right side with form */
.rightSide {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 50%;
  background: linear-gradient(135deg, #FFDEE9 0%, #B5FFFC 100%);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
}

.formContainer {
  max-width: 400px;
  margin: 0 auto;
}

.title {
  font-size: 1.5rem;
  font-weight: bold;
  text-align: center;
  color: #4f46e5;
  margin-bottom: 2rem;
  background: linear-gradient(90deg, #7abef7, #4080f5, #572ac2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
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
  font-size: 1rem;
}

.input {
  width: 100%;
  padding: 0.55rem;
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

/* Blinking cursor for auto-typing effect */
.cursor {
  display: inline-block;
  animation: blink 0.7s steps(1) infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0; }
}
