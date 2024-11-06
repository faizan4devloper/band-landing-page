/* EducationPage.module.css */
.educationPage {
  display: flex;
  padding: 0 60px;
  height: 100vh;
}

/* Sidebar.module.css */
.sidebar {
  width: 250px;
  background-color: #f7f7f7;
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
}

.sidebar li {
  padding: 10px 15px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.sidebar li.active {
  background-color: #e2d9fb;
}

.sidebar li:hover {
  background-color: #ddd;
}

.icon {
  margin-right: 10px;
}

/* MainContent.module.css */
.mainContent {
  flex: 1;
  padding: 20px;
  background-color: #fff;
}
