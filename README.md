<div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions
          </Link>
        </div>
.viewAllButton {
   font-weight: 600;
    font-size: 14px;
    color: #808080;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.6s ease;
  text-decoration: none;
}


.viewAllButton:hover {
  /*background-color: rgba(13, 85, 198, 1);*/
  transform: translateY(-2px);
  text-decoration: underline;
  color:#5f1ec1;
}
