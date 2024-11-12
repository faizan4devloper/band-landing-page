.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  background-color: #f0f4f8;
  max-height: 90vh; /* Set max height for vertical scrolling */
  overflow-y: auto; /* Enable scrolling */
  scrollbar-width: thin; /* Firefox */
  scrollbar-color: #0073e6 #f0f4f8;
}

/* Scrollbar for Webkit-based browsers (e.g., Chrome, Safari) */
.mainContent::-webkit-scrollbar {
  width: 8px;
}
.mainContent::-webkit-scrollbar-track {
  background: #f0f4f8;
}
.mainContent::-webkit-scrollbar-thumb {
  background-color: #0073e6;
  border-radius: 10px;
}

.section {
  background-color: #ffffff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  overflow-y: auto; /* Add scroll for long content */
  max-height: 300px; /* Prevent sections from expanding too much */
}

.section:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

h3 {
  margin-bottom: 15px;
  font-size: 1.5rem;
  color: #0073e6;
  font-weight: bold;
  text-align: center;
  border-bottom: 2px solid #0073e6;
  padding-bottom: 10px;
  margin-top: 0;
}

ul {
  padding-left: 20px;
  list-style-type: disc;
}

li {
  font-size: 0.95rem;
  color: #333;
  margin-bottom: 8px;
  line-height: 1.5;
}

a {
  color: #0073e6;
  text-decoration: none;
  transition: color 0.3s;
}

a:hover {
  color: #005bb5;
}

/* Add animations for list items */
li {
  opacity: 0;
  animation: fadeIn 0.5s ease forwards;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Custom styling for better text readability */
p, li, a {
  font-family: 'Arial', sans-serif;
}

/* Adding padding to improve section content spacing */
.section ul {
  padding-right: 10px;
}
