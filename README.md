/* Light theme */
:root {
  --sidebar-bg-light: rgba(255, 255, 255, 1);
  --sidebar-border-light: rgba(219, 197, 255, 1);
  --selected-filters-bg-light: rgba(230, 235, 245, 1);
  --selected-filter-color-light: #6f36cd;
  --category-header-bg-light: rgba(230, 235, 245, 1);
  --dropdown-bg-light: rgba(250, 250, 250, 1);
  --dropdown-item-hover-light: rgba(220, 220, 220, 1);
  --active-category-bg-light: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --active-item-bg-light: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --svg-icon-color-light: #6f36cd;
  --side-head-color-light: #808080;
}

/* Dark theme */
[data-theme='dark'] {
  --sidebar-bg-dark: rgba(30, 30, 30, 1);
  --sidebar-border-dark: rgba(60, 60, 60, 1);
  --selected-filters-bg-dark: rgba(50, 50, 50, 1);
  --selected-filter-color-dark: #bada55; /* Adjust as needed */
  --category-header-bg-dark: rgba(50, 50, 50, 1);
  --dropdown-bg-dark: rgba(40, 40, 40, 1);
  --dropdown-item-hover-dark: rgba(70, 70, 70, 1);
  --active-category-bg-dark: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --active-item-bg-dark: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --svg-icon-color-dark: #bada55; /* Adjust as needed */
  --side-head-color-dark: #b0b0b0;
}

.sidebar {
  position: fixed;
  top: 140px;
  left: 135px;
  width: 200px;
  height: calc(100% - 150px);
  border-right: 1px solid var(--sidebar-border-light); /* Default light theme border */
  background-color: var(--sidebar-bg-light); /* Default light theme background */
  padding: 0 20px;
  padding-right: 0px;
}

/* Dark theme styles */
[data-theme='dark'] .sidebar {
  background-color: var(--sidebar-bg-dark);
  border-right: 1px solid var(--sidebar-border-dark);
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
  background-color: var(--selected-filters-bg-light); /* Default light theme background */
  border-radius: 4px;
  padding: 5px 10px;
  font-size: 12px;
  color: var(--selected-filter-color-light); /* Default light theme color */
}

/* Dark theme styles */
[data-theme='dark'] .selectedFilter {
  background-color: var(--selected-filters-bg-dark);
  color: var(--selected-filter-color-dark);
}

.removeIcon {
  margin-left: 5px;
  cursor: pointer;
  color: var(--selected-filter-color-light); /* Default light theme color */
}

/* Dark theme styles */
[data-theme='dark'] .removeIcon {
  color: var(--selected-filter-color-dark);
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
  background-color: var(--category-header-bg-light); /* Default light theme background */
}

/* Dark theme styles */
[data-theme='dark'] .categoryHeader {
  background-color: var(--category-header-bg-dark);
}

.dropdown {
  padding: 10px;
  background-color: var(--dropdown-bg-light); /* Default light theme background */
  border-radius: 4px;
  margin-top: 10px;
  max-height: 150px;
  overflow-y: auto;
  
  scrollbar-width: thin;
  scrollbar-color: rgba(95, 30, 193, 0.8) transparent;
}

/* Dark theme styles */
[data-theme='dark'] .dropdown {
  background-color: var(--dropdown-bg-dark);
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
  background-color: var(--dropdown-item-hover-light); /* Default light theme hover */
  border-radius: 4px;
  color: #5F1EC1;
}

/* Dark theme styles */
[data-theme='dark'] .dropdownItem:hover {
  background-color: var(--dropdown-item-hover-dark);
}

.categoryHeader:not(.activeCategory):hover {
  background-color: var(--category-header-bg-light); /* Default light theme hover */
  border-radius: 8px 0 0 8px;
}

/* Dark theme styles */
[data-theme='dark'] .categoryHeader:not(.activeCategory):hover {
  background-color: var(--category-header-bg-dark);
}

.activeCategory {
  background: var(--active-category-bg-light); /* Default light theme background */
  color: white;
  border-radius: 8px 0 0 8px;
}

/* Dark theme styles */
[data-theme='dark'] .activeCategory {
  background: var(--active-category-bg-dark);
}

.activeItem {
  background: var(--active-item-bg-light); /* Default light theme background */
  color: white;
  border-radius: 4px;
}

/* Dark theme styles */
[data-theme='dark'] .activeItem {
  background: var(--active-item-bg-dark);
}

.svgIcon {
  width: 16px;
  height: 16px;
  fill: var(--svg-icon-color-light); /* Default light theme color */
}

/* Dark theme styles */
[data-theme='dark'] .svgIcon {
  fill: var(--svg-icon-color-dark);
}

.svgIcon:hover {
  fill: #5F1EC1;
}

.activeIcon {
  filter: brightness(0) invert(1);
}

.sideHead {
  font-size: 14px;
  font-weight: 500;
  margin-top: 0;
  color: var(--side-head-color-light); /* Default light theme color */
  position: relative;
  display: inline-block;
  padding-bottom: 5px;
}

.sideHead::after {
  content: '';
  display: block;
  width: 200px;
  height: 2px;
  background: var(--active-item-bg-light); /* Default light theme gradient */
  position: absolute;
  bottom: 0;
  left: 0;
}

/* Dark theme styles */
[data-theme='dark'] .sideHead {
  color: var(--side-head-color-dark);
}

[data-theme='dark'] .sideHead::after {
  background: var(--active-item-bg-dark);
}
