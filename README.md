.sidebar {
  position: fixed;
  top: 140px;
  left: 135px;
  width: 200px;
  height: calc(100% - 75px);
  border-right: 1px solid rgba(219, 197, 255, 1);
  padding: 0 20px;
  padding-right: 0px;
  overflow-y: auto; /* Enable scrolling for the sidebar */
}

.dropdown {
  padding: 10px;
  background-color: rgba(250, 250, 250, 1);
  border-radius: 4px;
  margin-top: 10px;
  max-height: 200px; /* Set a maximum height for the dropdown */
  overflow-y: auto; /* Enable scrolling for the dropdown */
}

.dropdownItem {
  display: flex;
  align-items: center;
  font-size: 11px;
  padding: 5px;
  cursor: pointer;
}

.checkbox {
  margin-right: 10px;
  width: 16px;
  height: 16px;
}

.itemText {
  margin-left: 5px;
}

.dropdownItem:hover {
  background-color: rgba(220, 220, 220, 1);
  border-radius: 4px;
  color: #5F1EC1;
}

.categoryHeader:not(.activeCategory):hover {
  background-color: rgba(230, 235, 245, 1);
  border-radius: 8px 0 0 8px;
}

.activeCategory {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border-radius: 8px 0 0 8px;
}

.activeItem {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border-radius: 4px;
}

.svgIcon {
  width: 16px;
  height: 16px;
}

.svgIcon:hover {
  fill: #5F1EC1;
}

.activeIcon {
  filter: brightness(0) invert(1);
}
