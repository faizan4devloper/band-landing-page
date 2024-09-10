.sideBar {
  width: 320px;
  min-width: 230px; /* Prevents shrinking */
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-right: 1px solid rgba(219, 197, 255, 1);
  overflow-y: auto;
  overscroll-behavior: contain;
  scroll-behavior: smooth;
  
}

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
  background-color: rgba(230, 235, 245, 1);
  border-radius: 8px 0 0 8px;
}

.active {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border-radius: 8px 0 0 8px;
}

.active .iconImg {
  filter: brightness(0) invert(1);
}

.iconImg {
  filter: brightness(0) invert(0);
}

.label {
  margin-right: auto;
  margin-left: 10px;
  font-size: 12px;
}
