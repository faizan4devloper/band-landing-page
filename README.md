/* Default Light Theme */
:root {
  --sidebar-bg: #ffffff;
  --sidebar-border: rgba(219, 197, 255, 1);
  --menu-item-bg-hover: rgba(230, 235, 245, 1);
  --menu-item-active-bg: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --menu-item-active-color: white;
  --icon-img-filter: brightness(0) invert(0);
  --icon-img-filter-active: brightness(0) invert(1); /* White for active icon */
  --text-color: #000000;
  --new-text-color: #000000;
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --sidebar-bg: #2c2c2c;
  --sidebar-border: rgba(50, 50, 50, 1);
  --menu-item-bg-hover: rgba(70, 70, 70, 1);
  --menu-item-active-bg: linear-gradient(90deg, #3a2d7f 0%, #2d5d8f 100%);
  --menu-item-active-color: #ffffff;
  --icon-img-filter: brightness(0) invert(1); /* Invert for dark theme */
  --icon-img-filter-active: brightness(1); /* Normal brightness for active icon in dark theme */
  --text-color: #ffffff;
  --new-text-color: #ffffff;
}

/* Sidebar Styling */
.sideBar {
  width: 320px;
  min-width: 230px;
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
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
  width: 100%;
  min-width: 200px;
  padding: 10px;
  margin: 5px 0;
  cursor: pointer;
  transition: background-color 0.3s;
  background: none;
  border: none;
  color: var(--new-text-color);
}

.menuItem:not(.active):hover {
  background-color: var(--menu-item-bg-hover);
  border-radius: 8px 0 0 8px;
}

.active {
  background: var(--menu-item-active-bg);
  color: var(--menu-item-active-color);
  border-radius: 8px 0 0 8px;
}

.active .iconImg {
  filter: var(--icon-img-filter-active); /* Use active filter */
}

.iconImg {
  filter: var(--icon-img-filter);
}

/* Adjust any other necessary styles to match the dark theme */
.label {
  margin-right: auto;
  margin-left: 10px;
  font-size: 12px;
}
