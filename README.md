Uncaught TypeError: Cannot read properties of undefined (reading 'policy_id')
    at ProductSheetsPage.js:413:1
    at Array.map (<anonymous>)
    at ProductSheetsPage.js:408:1
    at basicStateReducer (react-dom.development.js:15721:1)
    at updateReducer (react-dom.development.js:15845:1)
    at updateState (react-dom.development.js:16185:1)
    at Object.useState (react-dom.development.js:17096:1)
    at useState (react.development.js:1622:1)
    at ProductSheetsPage (ProductSheetsPage.js:328:1)
    at renderWithHooks (react-dom.development.js:15486:1)
(anonymous) @ ProductSheetsPage.js:413
(anonymous) @ ProductSheetsPage.js:408
basicStateReducer @ react-dom.development.js:15721
updateReducer @ react-dom.development.js:15845
updateState @ react-dom.development.js:16185
useState @ react-dom.development.js:17096
useState @ react.development.js:1622
ProductSheetsPage @ ProductSheetsPage.js:328
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
performWorkUntilDeadline @ scheduler.development.js:533Understand this errorAI
ProductSheetsPage.js:413 Uncaught TypeError: Cannot read properties of undefined (reading 'policy_id')
    at ProductSheetsPage.js:413:1
    at Array.map (<anonymous>)
    at ProductSheetsPage.js:408:1
    at basicStateReducer (react-dom.development.js:15721:1)
    at updateReducer (react-dom.development.js:15845:1)
    at updateState (react-dom.development.js:16185:1)
    at Object.useState (react-dom.development.js:17096:1)
    at useState (react.development.js:1622:1)
    at ProductSheetsPage (ProductSheetsPage.js:328:1)
    at renderWithHooks (react-dom.development.js:15486:1)
(anonymous) @ ProductSheetsPage.js:413
(anonymous) @ ProductSheetsPage.js:408
basicStateReducer @ react-dom.development.js:15721
updateReducer @ react-dom.development.js:15845
updateState @ react-dom.development.js:16185
useState @ react-dom.development.js:17096
useState @ react.development.js:1622
ProductSheetsPage @ ProductSheetsPage.js:328
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
performWorkUntilDeadline @ scheduler.development.js:533Understand this errorAI
react-dom.development.js:18704 The above error occurred in the <ProductSheetsPage> component:

    at ProductSheetsPage (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:2314:74)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50679:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:51413:5)
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:51347:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:49248:5)
    at App

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.
logCapturedError @ react-dom.development.js:18704
update.callback @ react-dom.development.js:18737
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
performWorkUntilDeadline @ scheduler.development.js:533Understand this errorAI
ProductSheetsPage.js:413 Uncaught TypeError: Cannot read properties of undefined (reading 'policy_id')
    at ProductSheetsPage.js:413:1
    at Array.map (<anonymous>)
    at ProductSheetsPage.js:408:1
    at basicStateReducer (react-dom.development.js:15721:1)
    at updateReducer (react-dom.development.js:15845:1)
    at updateState (react-dom.development.js:16185:1)
    at Object.useState (react-dom.development.js:17096:1)
    at useState (react.development.js:1622:1)
    at ProductSheetsPage (ProductSheetsPage.js:328:1)
    at renderWithHooks (react-dom.development.js:15486:1)
(anonymous) @ ProductSheetsPage.js:413
(anonymous) @ ProductSheetsPage.js:408
basicStateReducer @ react-dom.development.js:15721
updateReducer @ react-dom.development.js:15845
updateState @ react-dom.development.js:16185
useState @ react-dom.development.js:17096
useState @ react.development.js:1622
ProductSheetsPage @ ProductSheetsPage.js:328
renderWithHooks @ react-dom.development.js:15486
updateFunctionComponent @ react-dom.development.js:19617
beginWork @ react-dom.development.js:21640
beginWork$1 @ react-dom.development.js:27465
performUnitOfWork @ react-dom.development.js:26596
workLoopSync @ react-dom.development.js:26505
renderRootSync @ react-dom.development.js:26473
recoverFromConcurrentError @ react-dom.development.js:25889
performConcurrentWorkOnRoot @ react-dom.development.js:25789
workLoop @ scheduler.development.js:266
flushWork @ scheduler.development.js:239
performWorkUntilDeadline @ scheduler.development.js:533Understand this errorAI
