.searchBar {
  margin: 20px 0;
  display: flex;
  justify-content: center;
  position: absolute;
  bottom: 95%;
  left: 65%;
  align-items: center;
}

.inputWrapper {
  position: relative;
  width: 100%;
  max-width: 400px;
}

.searchInput {
  width: 70%;
  padding: 10px 16px 10px 40px; /* Add padding on the right for the icon */
  border: 1px solid #d3d3d3;
  border-radius: 50px;
  font-size: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  outline: none;
  transition: all 0.3s ease;
  position: relative;
}

.searchInput::placeholder {
  color: #888;
  font-size: 12px;
  opacity: 0.7;
}

.searchInput:focus {
  border-color: rgba(95, 30, 193, 1);
  box-shadow: 0 4px 8px rgba(95, 30, 193, 0.3);
  background-color: #fff;
  transform: scale(1.02);
}

.searchIcon {
  position: absolute;
  left: 16px;
  top: 50%;
  transform: translateY(-50%);
  color: #888;
  pointer-events: none; /* Allow clicks to pass through */
}
