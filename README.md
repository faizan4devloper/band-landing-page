const parseSkills = (text) => {
  if (!text) return [];

  // Convert keywords to lowercase for case-insensitive matching
  const lowerCaseKeywords = skillKeywords.map((keyword) => keyword.toLowerCase());

  // Split text based on word boundaries and punctuation
  return text.split(/[\s,.-]+/).map((word, index) => {
    const normalizedWord = word.trim().toLowerCase();

    if (lowerCaseKeywords.includes(normalizedWord)) {
      return (
        <span key={index} className={styles.skillBadge}>
          {word} {/* Display original word with correct case */}
        </span>
      );
    }
    return `${word} `;
  });
};
