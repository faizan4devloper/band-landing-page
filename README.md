import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync } from '@fortawesome/free-solid-svg-icons';

<button
  className={styles.reloadButton}
  onClick={() => handleReload("CL928697")}
  disabled={loading}
>
  <FontAwesomeIcon
    icon={faSync}
    spin={loading} // Spin when loading is true
    className={styles.icon}
  />
  {!loading && " Reload"} {/* Show text only when not loading */}
</button>



.reloadButton {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px; /* Space between icon and text */
  margin-top: 10px;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.reloadButton:hover {
  background: #0056b3; /* Darker blue on hover */
  transform: scale(1.05); /* Slight zoom effect */
}

.reloadButton:disabled {
  background-color: #ccc; /* Gray color when disabled */
  cursor: not-allowed;
}

.icon {
  font-size: 18px; /* Icon size */
}
