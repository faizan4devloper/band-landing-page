
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

/* Placeholder styling */
.searchInput::placeholder {
  color: #888;
  opacity: 0.7; /* Slightly faded placeholder */
  transition: opacity 0.3s ease, transform 0.3s ease; /* Smooth transition */
  animation: placeholderFadeIn 1s ease-in-out infinite; /* Add animation */
}

.searchInput:focus::placeholder {
  opacity: 0; /* Hide placeholder text on focus */
}

/* Placeholder animation */
@keyframes placeholderFadeIn {
  0% {
    opacity: 0;
    transform: scale(0.9);
  }
  50% {
    opacity: 0.5;
    transform: scale(1.05);
  }
  100% {
    opacity: 0;
    transform: scale(0.9);
  }
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3); /* Enhanced shadow on focus */
  background-color: #fff; /* Subtle background change */
  transform: scale(1.02); /* Slight scaling effect */
}
