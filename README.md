.sidebar {
  position: fixed;
  top: 110px;
  left: 30px;
  width: 250px;
  height: calc(100% - 75px);
  /*background-color: rgba(240, 240, 240, 1);*/
  border-right: 1px solid rgba(219, 197, 255, 1);
  /*box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);*/
  padding: 20px;
  overflow-y: auto;
}

.category {
  margin-bottom: 20px;
}

.categoryHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  padding: 10px;
  background-color: rgba(230, 230, 230, 1);
  border-radius: 4px;
}

.chevronIcon {
  margin-left: 10px;
}

.dropdown {
  padding: 10px;
  background-color: rgba(250, 250, 250, 1);
  border-radius: 4px;
  margin-top: 10px;
}

.dropdownItem {
  padding: 5px;
  cursor: pointer;
}

.dropdownItem:hover {
  background-color: rgba(220, 220, 220, 1);
}



/* SideBar.module.css */

.sideBar {
  width: 340px;
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-right: 1px solid rgba(219, 197, 255, 1);
  overflow-y: auto; /* Enable vertical scrolling */
  overscroll-behavior: contain; /* Prevent overscrolling */
  scroll-behavior: smooth; /* Enable smooth scrolling */
}

.menuItem {
  display: flex;
  flex-direction: row; /* Make the menu item a row layout */
  justify-content: space-between; /* Align items with space between */
  align-items: center;
  width: 100%;
  padding: 10px;
  margin: 5px 0px;
  cursor: pointer;
  transition: background-color 0.3s; /* Add transition for smoother effect */
  background: none;
  border: none;
}

.menuItem:not(.active):hover {
  background-color: rgba(230, 235, 245, 1);
  border-radius: 8px 0 0 8px;
}

.active {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border-radius: 8px 0 0 8px; /* Add border radius to the left side */
}

.active .iconImg {
  filter: brightness(0) invert(1); /* Active color - white */
}

.iconImg {
  filter: brightness(0) invert(0); /* Default color - black */
}

.label {
  margin-right: auto; /* Push label text to the right */
  margin-left: 10px; /* Add some space between icon and label */
  font-size: 12px;
}




