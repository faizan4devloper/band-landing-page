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

.rightIcon {
  display: flex;
  align-items: center;
  position: absolute;
  bottom: 1rem;
  right: 1rem;
  font-size: 1.5rem;
  color: #888;
  transition: transform 0.3s ease, color 0.3s ease;
  white-space: nowrap;
}

/* Icon should move to the right smoothly on hover */
.personaCard:hover .rightIcon {
  transform: translateX(15px); /* Moves the icon smoothly to the right */
  color: white; /* Change icon color */
}

.goText {
  margin-right: 0.5rem;
  opacity: 0;
  transform: translateX(-20px); /* Initially off-screen */
  transition: opacity 0.3s ease, transform 0.3s ease; /* Smooth transition */
  color: white;
}

/* Text slides in smoothly on hover */
.personaCard:hover .goText {
  opacity: 1; /* Makes text visible */
  transform: translateX(0); /* Slides text to its normal position */
}
