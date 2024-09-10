
/* Default Light Theme */
:root {
  --sidebar-bg: #ffffff;
  --sidebar-border: rgba(219, 197, 255, 1);
  --menu-item-bg-hover: rgba(230, 235, 245, 1);
  --menu-item-active-bg: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --menu-item-active-color: white;
  --icon-img-filter: brightness(0) invert(0);
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --sidebar-bg: #2c2c2c;
  --sidebar-border: rgba(50, 50, 50, 1);
  --menu-item-bg-hover: rgba(60, 60, 60, 1);
  --menu-item-active-bg: linear-gradient(90deg, #3a2d7f 0%, #2d5d8f 100%);
  --menu-item-active-color: #ffffff;
  --icon-img-filter: brightness(0) invert(1);
}

/* Sidebar Styling */
.sideBar {
  width: 320px;
  min-width: 230px; /* Prevents shrinking */
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  /*background-color: var(--sidebar-bg);*/
  border-right: 1px solid var(--sidebar-border);
  overflow-y: auto;
  overscroll-behavior: contain;
  scroll-behavior: smooth;
}

/* Menu Item Styling */
.menuItem {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 100%; /* Ensure full width */
  min-width: 200px; /* Prevent shrinking */
  padding: 10px;
  margin: 5px 0;
  cursor: pointer;
  transition: background-color 0.3s;
  background: none;
  border: none;
}

.menuItem:not(.active):hover {
  background-color: var(--menu-item-bg-hover);
  border-radius: 8px 0 0 8px;
}

.active {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--menu-item-active-color);
  border-radius: 8px 0 0 8px;
}

.active .iconImg {
  filter: var(--icon-img-filter);
}

.iconImg {
  filter: var(--icon-img-filter);
}

.label {
  margin-right: auto;
  margin-left: 10px;
  font-size: 12px;
}
