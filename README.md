Uncaught TypeError: contentData.map is not a function
    at MainContent (MainContent.js:41:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at updateFunctionComponent (react-dom.development.js:19617:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27490:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
    at workLoopSync (react-dom.development.js:26505:1)
MainContent @ MainContent.js:41
renderWithHooks @ react-dom.development.js:15486
updateFunctionComponent @ react-dom.development.js:19617
beginWork @ react-dom.development.js:21640
callCallback @ react-dom.development.js:4164
invokeGuardedCallbackDev @ react-dom.development.js:4213
invokeGuardedCallback @ react-dom.development.js:4277
beginWork$1 @ react-dom.development.js:27490
performUnitOfWork @ react-dom.development.js:26596
workLoopSync @ react-dom.development.js:26505
renderRootSync @ react-dom.development.js:26473
performConcurrentWorkOnRoot @ react-dom.development.js:25777
workLoop @ scheduler.development.js:266
flushWork @ scheduler.development.js:239
performWorkUntilDeadline @ scheduler.development.js:533
Show 14 more frames
Show less
MainContent.js:41 Uncaught TypeError: contentData.map is not a function
    at MainContent (MainContent.js:41:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at updateFunctionComponent (react-dom.development.js:19617:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27490:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
    at workLoopSync (react-dom.development.js:26505:1)
MainContent @ MainContent.js:41
renderWithHooks @ react-dom.development.js:15486
updateFunctionComponent @ react-dom.development.js:19617
beginWork @ react-dom.development.js:21640
callCallback @ react-dom.development.js:4164
invokeGuardedCallbackDev @ react-dom.development.js:4213
invokeGuardedCallback @ react-dom.development.js:4277
beginWork$1 @ react-dom.development.js:27490
performUnitOfWork @ react-dom.development.js:26596
workLoopSync @ react-dom.development.js:26505
renderRootSync @ react-dom.development.js:26473
recoverFromConcurrentError @ react-dom.development.js:25889
performConcurrentWorkOnRoot @ react-dom.development.js:25789
workLoop @ scheduler.development.js:266
flushWork @ scheduler.development.js:239
performWorkUntilDeadline @ scheduler.development.js:533
Show 15 more frames
Show less
react-dom.development.js:18704 The above error occurred in the <MainContent> component:

    at MainContent (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1045:88)
    at div
    at EducationPage (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:952:88)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:42705:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:43407:5)
    at div
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:43341:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:41278:5)
    at AuthProvider (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1751:3)
    at Provider (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:75454:3)
    at App (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:52:84)
    at Provider (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:75454:3)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.
logCapturedError @ react-dom.development.js:18704
(anonymous) @ react-dom.development.js:18737
callCallback @ react-dom.development.js:15036
commitUpdateQueue @ react-dom.development.js:15057
commitLayoutEffectOnFiber @ react-dom.development.js:23430
commitLayoutMountEffects_complete @ react-dom.development.js:24727
commitLayoutEffects_begin @ react-dom.development.js:24713
commitLayoutEffects @ react-dom.development.js:24651
commitRootImpl @ react-dom.development.js:26862
commitRoot @ react-dom.development.js:26721
finishConcurrentRender @ react-dom.development.js:25931
performConcurrentWorkOnRoot @ react-dom.development.js:25848
workLoop @ scheduler.development.js:266
flushWork @ scheduler.development.js:239
performWorkUntilDeadline @ scheduler.development.js:533
Show 15 more frames
Show less
MainContent.js:41 Uncaught TypeError: contentData.map is not a function
    at MainContent (MainContent.js:41:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at updateFunctionComponent (react-dom.development.js:19617:1)
    at beginWork (react-dom.development.js:21640:1)
    at beginWork$1 (react-dom.development.js:27465:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
    at workLoopSync (react-dom.development.js:26505:1)
    at renderRootSync (react-dom.development.js:26473:1)
    at recoverFromConcurrentError (react-dom.development.js:25889:1)
    at performConcurrentWorkOnRoot (react-dom.development.js:25789:1)



    

import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css'; // Assuming your CSS module for styling
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [openQuestion, setOpenQuestion] = useState(null);

  // New message to send in the query parameter
  const newMessage = 'what are the average class sizes and student-teacher ratios in the local schools?';

  // Fetch data from API using GET request
  const fetchData = async () => {
    try {
      // Sending the question as a query parameter
      const response = await axios.get('https://2kn1kfoouh.execute-api.us-east-1.amazonaws.com/edu/cit-adv2', { 
        params: { question: newMessage }, // 'question' is sent as a URL query parameter
        headers: {
          'Content-Type': 'application/json'
        }
      });
      setContentData(response.data); // Assuming the API response is structured as expected
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  const toggleAnswer = (index) => {
    setOpenQuestion(openQuestion === index ? null : index);
  };

  return (
    <div className={styles.mainContent}>
      {openQuestion === null ? (
        contentData.map((item, index) => (
          <div key={index} className={styles.questionBlock}>
            <div
              className={styles.question}
              onClick={() => toggleAnswer(index)}
            >
              {item.question}
              <FontAwesomeIcon
                icon={faChevronDown}
                className={styles.chevronIcon}
              />
            </div>
          </div>
        ))
      ) : (
        <div className={styles.questionBlock}>
          <div
            className={styles.question}
            onClick={() => toggleAnswer(openQuestion)}
          >
            {contentData[openQuestion].question}
            <FontAwesomeIcon
              icon={faChevronUp}
              className={styles.chevronIcon}
            />
          </div>
          {/* Display grid layout for the first question with multiple layouts */}
          {openQuestion === 0 ? (
            <div className={styles.gridAnswer}>
              <div className={styles.gridItem}>
                <h3>Textual Response</h3>
                <p>{contentData[0]?.answer?.textual || 'No Textual Response Available'}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Citizen Experience</h3>
                <p>{contentData[0]?.answer?.citizenExperience || 'No Citizen Experience Available'}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Factual Info</h3>
                <p>{contentData[0]?.answer?.factualInfo || 'No Factual Info Available'}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Contextual</h3>
                <p>{contentData[0]?.answer?.contextual || 'No Contextual Info Available'}</p>
              </div>
            </div>
          ) : (
            <div className={styles.answer}>
              {contentData[openQuestion].answer || 'No answer available'}
            </div>
          )}
        </div>
      )}
    </div>
  );
};

export default MainContent;
