Uncaught TypeError: Cannot read properties of undefined (reading 'map')
    at MainContent (MainContent.js:30:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at updateFunctionComponent (react-dom.development.js:19617:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27490:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
    at workLoopSync (react-dom.development.js:26505:1)
MainContent @ MainContent.js:30
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

    at MainContent (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:2239:74)
    at div
    at JobPage
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:45559:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:46261:5)
    at div
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:46195:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:44132:5)
    at AuthProvider (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:2851:3)
    at Provider (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:78557:3)
    at App (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50:84)
    at Provider (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:78557:3)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.
