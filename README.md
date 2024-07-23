.searchBar {
  margin: 20px 0;
  display: flex;
  justify-content: center;
}

.searchInput {
  width: 100%;
  max-width: 400px;
  padding: 12px 16px;
  border: 1px solid #d3d3d3;
  border-radius: 50px; /* Rounded corners */
  font-size: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  outline: none;
  transition: all 0.3s ease; /* Smooth transition */
}

/* Placeholder styling with animation */
.searchInput::placeholder {
  color: #888;
  opacity: 0.7; /* Slightly faded placeholder */
  transition: opacity 0.3s ease, transform 0.3s ease; /* Smooth transition */
  animation: placeholderPulse 2s ease-in-out infinite; /* Pulsing animation */
}

/* Placeholder animation: pulsing */
@keyframes placeholderPulse {
  0%, 100% {
    opacity: 0.7;
    transform: scale(1);
  }
  50% {
    opacity: 1;
    transform: scale(1.05);
  }
}

/* Placeholder animation: slide up */
@keyframes placeholderSlideUp {
  0% {
    opacity: 0;
    transform: translateY(10px);
  }
  50% {
    opacity: 0.5;
    transform: translateY(0);
  }
  100% {
    opacity: 0;
    transform: translateY(-10px);
  }
}

.searchInput:focus::placeholder {
  opacity: 0; /* Hide placeholder text on focus */
  animation: placeholderSlideUp 0.5s ease-out; /* Slide-up animation on focus */
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3); /* Enhanced shadow on focus */
  background-color: #fff; /* Subtle background change */
  transform: scale(1.02); /* Slight scaling effect */
}







.searchBar {
  margin: 20px 0;
  display: flex;
  justify-content: center;
}

.searchInput {
  width: 100%;
  max-width: 400px;
  padding: 12px 16px;
  border: 1px solid #d3d3d3;
  border-radius: 50px; /* Rounded corners */
  font-size: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  outline: none;
  transition: all 0.3s ease; /* Smooth transition */
  position: relative;
  overflow: hidden; /* Ensure no overflow of typewriter effect */
}

/* Placeholder styling with typewriter effect */
.searchInput::placeholder {
  color: #888;
  opacity: 0.7; /* Slightly faded placeholder */
  white-space: nowrap; /* Prevent text wrapping */
  overflow: hidden; /* Hide text that overflows */
  animation: typing 4s steps(30, end), blink-caret 0.75s step-end infinite; /* Apply typewriter animation */
}

/* Typewriter effect */
@keyframes typing {
  from {
    width: 0; /* Start with zero width */
  }
  to {
    width: 100%; /* Expand to full width */
  }
}

/* Caret blinking animation */
@keyframes blink-caret {
  from, to {
    border-color: transparent; /* Invisible caret */
  }
  50% {
    border-color: rgba(95, 30, 193, 1); /* Visible caret */
  }
}

.searchInput:focus::placeholder {
  opacity: 0; /* Hide placeholder text on focus */
  animation: none; /* Stop animation when focused */
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3); /* Enhanced shadow on focus */
  background-color: #fff; /* Subtle background change */
  transform: scale(1.02); /* Slight scaling effect */
}
