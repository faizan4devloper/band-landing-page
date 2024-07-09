<div className={styles.formGroup}>
  <label>GenAI Solution Name</label>
  <Select
    options={genAISolutions}
    value={selectedSolution}
    onChange={setSelectedSolution}
    className={styles.select}
    classNamePrefix="select"
    placeholder="Select a solution"
    isClearable
  />
</div>


/* Custom styles for react-select */
.select__control {
  border: 1px solid #ccc;
  border-radius: 4px;
  font-family: "Poppins", sans-serif;
  font-size: 12px; /* Ensure control text is 12px */
  color: #333;
}

.select__control--is-focused {
  border-color: #5f1ec1;
  box-shadow: 0 0 0 1px #5f1ec1;
}

.select__menu {
  font-size: 12px; /* Ensure menu text is 12px */
}

.select__option {
  padding: 8px 8px;
  font-size: 12px; /* Ensure option text is 12px */
}

.select {
  width: 410px;
}

/* Ensure focused option background color */
.select__option--is-focused {
  background-color: rgba(95, 30, 193, 0.1);
}

/* Ensure selected option background color and text color */
.select__option--is-selected {
  background-color: rgba(95, 30, 193, 0.8);
  color: #fff;
}

/* Custom style for placeholder text */
.select__placeholder {
  font-size: 12px; /* Ensure placeholder text is 12px */
}
