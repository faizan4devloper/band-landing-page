.Personas {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: rgba(255, 255, 255, 1); /* Keeping white background */
}

.gradientText {
  background: -webkit-gradient(linear, left top, right top, 
    color-stop(-19.51%, #7abef7), 
    color-stop(36.51%, #4080f5), 
    to(#572ac2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent; /* For compatibility with non-Webkit browsers */
}

.personaCards {
  display: flex;
  gap: 2rem;
  justify-content: center;
  position: relative;
}

.personaCard {
  width: 240px;
  padding: 2rem;
  border-radius: 0.75rem;
  text-align: center;
  background-color: #f2f2f2; /* Updated based on persona theme */
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, background-color 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  overflow: hidden;
  position: relative;
}

.personaCard:hover {
  transform: translateY(-8px);
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: white;
  box-shadow: 0 20px 25px rgba(0, 0, 0, 0.15);
}

.personaCard h2 {
  font-size: 1.8rem;
  margin-bottom: 1rem;
  background: -webkit-gradient(linear, left top, right top, 
    color-stop(-19.51%, #7abef7), 
    color-stop(36.51%, #4080f5), 
    to(#572ac2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent; /* For compatibility with non-Webkit browsers */
}

/* Change gradient text to white on hover */
.personaCard:hover h2 {
  color: white; /* Change text color to white */
}

.personaCard .description {
  font-size: 1rem;
  margin-bottom: 1rem;
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.3s ease, transform 0.3s ease;
  color: #555;
}

.personaCard:hover .description {
  opacity: 1;
  transform: translateY(0);
  color: white;
}

/* Style for the right icon and text */
.rightIcon {
  display: flex;
  align-items: center;
  position: absolute;
  bottom: 1rem;
  left: 1rem; /* Set to left to start on the left side */
  font-size: 1.5rem;
  transition: color 0.3s ease;
  white-space: nowrap;
  background: -webkit-gradient(linear, left top, right top, 
    color-stop(-19.51%, #7abef7), 
    color-stop(36.51%, #4080f5), 
    to(#572ac2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent; /* For compatibility with non-Webkit browsers */
}

/* Arrow will move on hover */
.rightIcon .arrow {
  margin-left: 0.5rem; /* Space between text and arrow */
  transition: transform 0.3s ease; /* Smooth transition */
}

.goText {
  opacity: 0; /* Initially hidden */
  transform: translateX(-20px); /* Initially moved to the left */
  transition: opacity 0.3s ease, transform 0.3s ease; /* Smooth transition */
  color: white; /* Change text color */
}

/* Text appears on hover with sliding effect */
.personaCard:hover .goText {
  opacity: 1; /* Makes text visible */
  transform: translateX(0); /* Moves text to its original position */
}

/* Move the arrow to the right on hover */
.personaCard:hover .arrow {
  transform: translateX(10px); /* Adjust to create a sliding effect */
}

/* Change icon color on hover */
.personaCard:hover .rightIcon {
  color: white; /* Change icon color to white on hover */
}
