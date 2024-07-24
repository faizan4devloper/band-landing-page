/* Sidebar Scrollbar */
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

/* Custom Scrollbars for Sidebar */
.sidebar::-webkit-scrollbar {
  width: 8px; /* Width of the scrollbar */
}

.sidebar::-webkit-scrollbar-thumb {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  border-radius: 4px;
}

.sidebar::-webkit-scrollbar-track {
  background: rgba(230, 235, 245, 1);
  border-radius: 4px;
}

.sidebar {
  scrollbar-width: thin;
  scrollbar-color: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%) rgba(230, 235, 245, 1);
}

/* Custom Scrollbars for Dropdown */
.dropdown {
  padding: 10px;
  background-color: rgba(250, 250, 250, 1);
  border-radius: 4px;
  margin-top: 10px;
  max-height: 150px; /* Set a maximum height for the dropdown */
  overflow-y: auto; /* Enable scrolling for the dropdown */
}

.dropdown::-webkit-scrollbar {
  width: 8px; /* Width of the scrollbar */
}

.dropdown::-webkit-scrollbar-thumb {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  border-radius: 4px;
}

.dropdown::-webkit-scrollbar-track {
  background: rgba(230, 235, 245, 1);
  border-radius: 4px;
}

.dropdown {
  scrollbar-width: thin;
  scrollbar-color: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%) rgba(230, 235, 245, 1);
}

.selectedFilters {
  margin-bottom: 20px;
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

.selectedFilter {
  display: flex;
  align-items: center;
  background-color: rgba(230, 235, 245, 1);
  border-radius: 4px;
  padding: 5px 10px;
  font-size: 12px;
  color: #6f36cd;
}

.removeIcon {
  margin-left: 5px;
  cursor: pointer;
  color: #6f36cd;
}

.removeIcon:hover {
  color: #d9534f; /* Adjust the color on hover */
}

.category {
  margin-bottom: 20px;
}

.categoryHeader {
  display: flex;
  align-items: center;
  cursor: pointer;
  font-size: 12px;
  padding: 10px 15px;
  border-radius: 8px 0 0 8px;
  background-color: rgba(230, 235, 245, 1);
}

.categoryHeader img {
  margin-right: 8px;
}

.chevronIcon {
  margin-left: auto;
}

.dropdown {
  padding: 10px;
  background-color: rgba(250, 250, 250, 1);
  border-radius: 4px;
  margin-top: 10px;
  max-height: 150px; /* Set a maximum height for the dropdown */
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

