.sidebar {
  position: fixed;
  top: 110px;
  left: 135px;
  width: 200px;
  height: calc(100% - 75px);
  border-right: 1px solid rgba(219, 197, 255, 1);
  padding: 20px;
  padding-right: 0px;
  overflow-y: auto;
}

.category {
  margin-bottom: 20px;
}

.sideHead {
  font-size: 16px;
  font-weight: 500;
  margin-top: 0;
  margin-left: 15px;
  color: #6f36cd; /* Adjust the color as desired */
  position: relative;
  display: inline-block;
  padding-bottom: 5px;
}

.sideHead::after {
  content: '';
  display: block;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  position: absolute;
  bottom: 0;
  left: 0;
}

.categoryHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  font-size: 12px;
  padding: 10px 15px;
  border-radius: 8px 0 0 8px;
  background-color: rgba(230, 235, 245, 1);
  transition: background-color 0.3s, color 0.3s;
}

.categoryHeader:hover {
  background-color: rgba(220, 230, 240, 1);
  color: #5F1EC1;
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
  display: flex;
  align-items: center;
  font-size: 11px;
  padding: 5px;
  cursor: pointer;
  text-align: left;
  transition: background-color 0.3s, color 0.3s;
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
  background-color: rgba(220, 230, 240, 1);
  border-radius: 8px 0 0 8px;
  color: #5F1EC1;
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
  width: 20px;
  height: 20px;
  transition: transform 0.2s, fill 0.2s; /* Add transition for smooth effect */
}

.categoryHeader:hover .svgIcon,
.activeCategory .svgIcon {
  transform: scale(1.1); /* Slightly enlarge on hover and active */
  fill: #5F1EC1; /* Change color on hover and active */
}
