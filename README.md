{file && file.type === 'application/pdf' && (
  <object
    data={fileContent}
    type="application/pdf"
    className={styles.pdfPreview}
    width="100%"
    height="500px"
  >
    <p>It appears your browser does not support PDFs. You can download the file to view it: <a href={fileContent} download>Download PDF</a>.</p>
  </object>
)}
