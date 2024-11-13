/* Style the overall container */
.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  overflow-y: auto;
  scrollbar-width: thin; /* For Firefox */
  scrollbar-color: #0073e6 #f1f1f1; /* For Firefox */
}

/* Style the section container */
.section {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  height: 280px;
  overflow-y: auto;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  scrollbar-width: thin; /* For Firefox */
  scrollbar-color: #0073e6 #f1f1f1; /* For Firefox */
}

/* Hover effect on section */
.section:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

h3 {
  margin-bottom: 15px;
  font-size: 1.2rem;
  color: #333;
}

ul {
  padding-left: 0;
  list-style-type: none;
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.skillList {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.skillItem {
  background-color: #0073e6;
  color: #fff;
  padding: 6px 12px;
  border-radius: 4px;
  font-size: 0.8rem;
  font-weight: 500;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.skillItem:hover {
  background-color: #005bb5;
  transform: scale(1.05);
}

/* Custom Scrollbar for Webkit Browsers (Chrome, Safari) */
.section::-webkit-scrollbar {
  width: 8px;
}

.section::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}

.section::-webkit-scrollbar-thumb {
  background-color: #0073e6;
  border-radius: 10px;
  border: 2px solid #f1f1f1;
}

.section::-webkit-scrollbar-thumb:hover {
  background-color: #005bb5;
}

/* For the mainContent scroll */
.mainContent::-webkit-scrollbar {
  width: 8px;
}

.mainContent::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}

.mainContent::-webkit-scrollbar-thumb {
  background-color: #0073e6;
  border-radius: 10px;
  border: 2px solid #f1f1f1;
}

.mainContent::-webkit-scrollbar-thumb:hover {
  background-color: #005bb5;
}

/* For Firefox scrollbar styling */
.scrollbarCustom {
  scrollbar-width: thin;
  scrollbar-color: #0073e6 #f1f1f1;
}
