@keyframes placeholderAnimation {
  0% {
    color: #ccc;
  }
  50% {
    color: #999;
  }
  100% {
    color: #ccc;
  }
}

.searchBar {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px 0;
}

.searchInput {
  width: 100%;
  max-width: 400px;
  padding: 10px 15px;
  border: 1px solid #ccc;
  border-radius: 25px;
  font-size: 16px;
  transition: all 0.3s ease;
}

.searchInput:focus {
  border-color: #007BFF;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
  outline: none;
}

.searchInput::placeholder {
  animation: placeholderAnimation 2s infinite;
}
