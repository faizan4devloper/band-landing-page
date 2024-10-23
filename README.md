<div className={styles.personaCards}>
  {personas.map((persona, index) => (
    <div
      key={index}
      className={`${styles.personaCard} ${index === currentPersonaIndex ? styles.active : ''}`}
      style={{ backgroundColor: persona.themeColor }}
      onClick={() => handleClick(persona.route)}
    >
      <img src={persona.image} alt={`${persona.title} icon`} className={styles.personaImage} /> {/* Image for the persona */}
      <h2 className={styles.cardHead}>{persona.title}</h2>
      <p className={styles.description}>{persona.description}</p>
      <div className={styles.rightIcon}>
        <span className={styles.goText}>Explore</span>
        <FontAwesomeIcon icon={faArrowRight} className={styles.arrow} />
      </div>
    </div>
  ))}
</div>


.personaImage {
  width: 80px; /* Adjust width as needed */
  height: auto; /* Maintain aspect ratio */
  margin-bottom: 1rem; /* Space between image and title */
  border-radius: 50%; /* Make it circular or adjust as needed */
  transition: transform 0.3s ease; /* Add a transition for hover effect */
}

.personaCard:hover .personaImage {
  transform: scale(1.1); /* Slightly enlarge the image on hover */
}
