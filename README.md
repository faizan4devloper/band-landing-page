Uncaught TypeError: Cannot destructure property 'setHeaderZIndex' of '(0 , _Context_HeaderContext__WEBPACK_IMPORTED_MODULE_7__.useHeaderContext)(...)' as it is undefined.
    at SideBarPage (SideBarPage.js:18:1)
    at renderWithHooks (react-dom.development.js:16305:1)
    at mountIndeterminateComponent (react-dom.development.js:20074:1)
    at beginWork (react-dom.development.js:21587:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27451:1)
    at performUnitOfWork (react-dom.development.js:26557:1)
    at workLoopSync (react-dom.development.js:26466:1)
SideBarPage @ SideBarPage.js:18
renderWithHooks @ react-dom.development.js:16305
mountIndeterminateComponent @ react-dom.development.js:20074
beginWork @ react-dom.development.js:21587
callCallback @ react-dom.development.js:4164
invokeGuardedCallbackDev @ react-dom.development.js:4213
invokeGuardedCallback @ react-dom.development.js:4277
beginWork$1 @ react-dom.development.js:27451
performUnitOfWork @ react-dom.development.js:26557
workLoopSync @ react-dom.development.js:26466
renderRootSync @ react-dom.development.js:26434
performSyncWorkOnRoot @ react-dom.development.js:26085
flushSyncCallbacks @ react-dom.development.js:12042
(anonymous) @ react-dom.development.js:25651
Show 13 more frames
Show less
SideBarPage.js:18 Uncaught TypeError: Cannot destructure property 'setHeaderZIndex' of '(0 , _Context_HeaderContext__WEBPACK_IMPORTED_MODULE_7__.useHeaderContext)(...)' as it is undefined.
    at SideBarPage (SideBarPage.js:18:1)
    at renderWithHooks (react-dom.development.js:16305:1)
    at mountIndeterminateComponent (react-dom.development.js:20074:1)
    at beginWork (react-dom.development.js:21587:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27451:1)
    at performUnitOfWork (react-dom.development.js:26557:1)
    at workLoopSync (react-dom.development.js:26466:1)
SideBarPage @ SideBarPage.js:18
renderWithHooks @ react-dom.development.js:16305
mountIndeterminateComponent @ react-dom.development.js:20074
beginWork @ react-dom.development.js:21587
callCallback @ react-dom.development.js:4164
invokeGuardedCallbackDev @ react-dom.development.js:4213
invokeGuardedCallback @ react-dom.development.js:4277
beginWork$1 @ react-dom.development.js:27451
performUnitOfWork @ react-dom.development.js:26557
workLoopSync @ react-dom.development.js:26466
renderRootSync @ react-dom.development.js:26434
recoverFromConcurrentError @ react-dom.development.js:25850
performSyncWorkOnRoot @ react-dom.development.js:26096
flushSyncCallbacks @ react-dom.development.js:12042
(anonymous) @ react-dom.development.js:25651
Show 14 more frames
Show less
react-dom.development.js:18687 The above error occurred in the <SideBarPage> component:

    at SideBarPage (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:5124:84)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:51731:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:52422:5)
    at div
    at MainApp (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:767:82)
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:52356:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50309:5)
    at App

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.

// src/components/Sidebar/SideBarPage.js
import React, { useState, useEffect } from 'react';
import { useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import Header from '../Header/Header';
import SideBar from './SideBar';
import MainContent from './MainContent';
import styles from './SideBarPage.module.css';
import { getCardsData } from '../../data';
import { useHeaderContext } from '../Context/HeaderContext'; // Import context

const SideBarPage = () => {
  const [activeTab, setActiveTab] = useState('description');
  const [cardContent, setCardContent] = useState({});
  const [cardTitle, setCardTitle] = useState('');
  const location = useLocation();
  const { setHeaderZIndex } = useHeaderContext(); // Use context

  useEffect(() => {
    const fetchData = async () => {
      const data = await getCardsData();
      const params = new URLSearchParams(location.search);
      const title = params.get('title');
      if (title) {
        setCardTitle(title);
        const card = data.find((c) => c.title === title);
        if (card) {
          setCardContent(card.content);
        }
      }
    };

    fetchData();
  }, [location.search]);

  useEffect(() => {
    setHeaderZIndex(0); // Set zIndex to 0 for this page
    return () => setHeaderZIndex(1000); // Reset zIndex on unmount
  }, [setHeaderZIndex]);

  const handleTabChange = (tabId) => {
    setActiveTab(tabId);
  };

  const handleBackButtonClick = () => {
    window.history.back();
  };

  return (
    <div className={styles.sideBarPage}>
      <Header />
      <div className={styles.header2}>
        <button onClick={handleBackButtonClick} className={styles.backButton}>
          <FontAwesomeIcon icon={faArrowLeft} />
        </button>
        {cardTitle && <div className={styles.cardTitle}>{cardTitle}</div>}
      </div>
      <div className={styles.contentWrapper}>
        <SideBar activeTab={activeTab} handleTabChange={handleTabChange} />
        <MainContent activeTab={activeTab} content={cardContent} />
      </div>
    </div>
  );
};

export default SideBarPage;
