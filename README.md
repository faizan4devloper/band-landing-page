Uncaught TypeError: videoRef.current.play is not a function
    at Home.js:62:1
    at commitHookEffectListMount (react-dom.development.js:23150:1)
    at invokePassiveEffectMountInDEV (react-dom.development.js:25154:1)
    at invokeEffectsInDev (react-dom.development.js:27351:1)
    at commitDoubleInvokeEffectsInDEV (react-dom.development.js:27330:1)
    at flushPassiveEffectsImpl (react-dom.development.js:27056:1)
    at flushPassiveEffects (react-dom.development.js:26984:1)
    at performSyncWorkOnRoot (react-dom.development.js:26076:1)
    at flushSyncCallbacks (react-dom.development.js:12042:1)
    at commitRootImpl (react-dom.development.js:26959:1)
2react-dom.development.js:18687 The above error occurred in the <Home> component:

    at Home (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1064:3)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50022:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50713:5)
    at div
    at MainApp (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:645:81)
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50647:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:48600:5)
    at App

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries
