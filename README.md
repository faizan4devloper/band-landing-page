Uncaught ReferenceError: styles is not defined
    at FilePreviewModal (main.096e9791058304485c8a.hot-update.js:54:16)
    at renderWithHooks (react-dom.development.js:15194:1)
    at updateFunctionComponent (react-dom.development.js:19330:1)
    at beginWork (react-dom.development.js:21350:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:3876:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:3928:1)
    at invokeGuardedCallback (react-dom.development.js:3994:1)
    at beginWork$1 (react-dom.development.js:27188:1)
    at performUnitOfWork (react-dom.development.js:26307:1)
    at workLoopSync (react-dom.development.js:26219:1)
FilePreviewModal @ main.096e9791058304485c8a.hot-update.js:54
renderWithHooks @ react-dom.development.js:15194
updateFunctionComponent @ react-dom.development.js:19330
beginWork @ react-dom.development.js:21350
callCallback @ react-dom.development.js:3876
invokeGuardedCallbackDev @ react-dom.development.js:3928
invokeGuardedCallback @ react-dom.development.js:3994
beginWork$1 @ react-dom.development.js:27188
performUnitOfWork @ react-dom.development.js:26307
workLoopSync @ react-dom.development.js:26219
renderRootSync @ react-dom.development.js:26189
recoverFromConcurrentError @ react-dom.development.js:25600
performSyncWorkOnRoot @ react-dom.development.js:25840
flushSyncCallbacks @ react-dom.development.js:11740
flushSync @ react-dom.development.js:25947
scheduleRefresh @ react-dom.development.js:27542
(anonymous) @ react-refresh-runtime.development.js:12
performReactRefresh @ react-refresh-runtime.development.js:4
(anonymous) @ index.es.js:270
setTimeout
enqueueUpdate @ index.es.js:266
executeRuntime @ index.es.js:418
$ReactRefreshModuleRuntime$ @ main.096e9791058304485c8a.hot-update.js:134
./src/components/Pages/Job/FilePreviewModal.js @ main.096e9791058304485c8a.hot-update.js:147
options.factory @ create fake namespace object:24
__webpack_require__ @ tslib.es6.mjs:56
_requireSelf @ tslib.es6.mjs:233
apply @ hot module replacement:339
(anonymous) @ bootstrap:17
internalApply @ bootstrap:15
(anonymous) @ tslib.es6.mjs:348
waitForBlockingPromises @ tslib.es6.mjs:310
(anonymous) @ tslib.es6.mjs:346
Promise.then
(anonymous) @ tslib.es6.mjs:345
Promise.then
(anonymous) @ tslib.es6.mjs:328
Promise.then
hotCheck @ tslib.es6.mjs:317
check @ log.js:1
(anonymous) @ log.js:34
emit @ index.js:31
reloadApp @ createSocketURL.js:68
warnings @ index.js:24
(anonymous) @ state-machine.js:24
client.onmessage @ index.js:7
Show 35 more frames
Show less
main.096e9791058304485c8a.hot-update.js:134 The above error occurred in the <FilePreviewModal> component:

    at FilePreviewModal (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/main.096e9791058304485c8a.hot-update.js:28:3)
    at div
    at Sidebar (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/main.26205e998adfbd582aaa.hot-update.js:185:74)
    at div
    at JobPage
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:45121:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:45823:5)
    at div
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:45757:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:43694:5)
    at AuthProvider (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:2413:3)
    at Provider (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:77550:3)
    at App (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50:84)
    at Provider (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:77550:3)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.





import React, { useState, useEffect } from 'react';
import * as pdfjsLib from 'pdfjs-dist';
import 'pdfjs-dist/build/pdf.worker.entry';
import styles from './FilePreviewModal.module.css';

const FilePreviewModal = ({ file, closeModal }) => {
  const [pdfPages, setPdfPages] = useState([]);

  const loadPdf = async (file) => {
    try {
      const pdfData = await file.arrayBuffer(); // Convert file to ArrayBuffer
      const pdf = await pdfjsLib.getDocument(pdfData).promise; // Load PDF
      const pages = [];

      // Render each page as an image
      for (let pageNum = 1; pageNum <= pdf.numPages; pageNum++) {
        const page = await pdf.getPage(pageNum);
        const viewport = page.getViewport({ scale: 1.5 }); // Adjust scale as needed
        const canvas = document.createElement('canvas');
        const context = canvas.getContext('2d');
        canvas.height = viewport.height;
        canvas.width = viewport.width;

        await page.render({ canvasContext: context, viewport }).promise;
        pages.push(canvas.toDataURL('image/png')); // Store rendered page as an image
      }

      setPdfPages(pages);
    } catch (error) {
      console.error('Error loading PDF:', error);
    }
  };

  useEffect(() => {
    if (file && file.type === 'application/pdf') {
      loadPdf(file);
    }
  }, [file]);

  return (
    <div className={styles.modalOverlay} onClick={closeModal}>
      <div className={styles.modalContent} onClick={(e) => e.stopPropagation()}>
        <h2>PDF Preview</h2>
        <div className={styles.pdfContainer}>
          {pdfPages.length > 0 ? (
            pdfPages.map((pageSrc, index) => (
              <img
                key={index}
                src={pageSrc}
                alt={`PDF page ${index + 1}`}
                className={styles.pdfPageImage}
              />
            ))
          ) : (
            <p>Loading PDF...</p>
          )}
        </div>
        <button onClick={closeModal} className={styles.closeButton}>Close</button>
      </div>
    </div>
  );
};

export default FilePreviewModal;
