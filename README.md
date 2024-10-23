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

.personaCard .description {
  font-size: 1rem;
  margin-bottom: 1rem;
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.3s ease, transform 0.3s ease;
  color: #555;
}

.personaCard:hover .description, .cardHead {
  opacity: 1;
  transform: translateY(0);
  color: white;
}

.rightIcon {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  position: absolute;
  bottom: 1rem;
  right: 1.5rem;
  font-size: 1.5rem;
  color: #888;
  opacity: 0;
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.personaCard:hover .rightIcon {
  opacity: 1;
  color: #fff;
  transform: translateX(5px); /* Slide in effect */
}

