<button
            className={styles.previewButton}
            onClick={() => {
              console.log("Preview Link:", doc.S); // Debugging
              if (doc.S) {
                openModal(doc.S);
              } else {
                console.error("No preview link available for this document");
              }
            }}
          >
            View Document
          </button>
