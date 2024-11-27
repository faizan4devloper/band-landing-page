Uncaught Error: Objects are not valid as a React child (found: object with keys {claimid, emailbody}). If you meant to render a collection of children, use an array instead.
    at throwOnInvalidObjectType (react-dom.development.js:13123:1)
    at reconcileChildFibers (react-dom.development.js:14064:1)
    at reconcileChildren (react-dom.development.js:19193:1)
    at updateHostComponent (react-dom.development.js:19953:1)
    at beginWork (react-dom.development.js:21657:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27490:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
throwOnInvalidObjectType @ react-dom.development.js:13123
reconcileChildFibers @ react-dom.development.js:14064
reconcileChildren @ react-dom.development.js:19193
updateHostComponent @ react-dom.development.js:19953
beginWork @ react-dom.development.js:21657
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
react-dom.development.js:13123 Uncaught Error: Objects are not valid as a React child (found: object with keys {claimid, emailbody}). If you meant to render a collection of children, use an array instead.
    at throwOnInvalidObjectType (react-dom.development.js:13123:1)
    at reconcileChildFibers (react-dom.development.js:14064:1)
    at reconcileChildren (react-dom.development.js:19193:1)
    at updateHostComponent (react-dom.development.js:19953:1)
    at beginWork (react-dom.development.js:21657:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27490:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
throwOnInvalidObjectType @ react-dom.development.js:13123
reconcileChildFibers @ react-dom.development.js:14064
reconcileChildren @ react-dom.development.js:19193
updateHostComponent @ react-dom.development.js:19953
beginWork @ react-dom.development.js:21657
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
react-dom.development.js:18704 The above error occurred in the <p> component:

    at p
    at div
    at div
    at div
    at GenerateEmail (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1050:80)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:51702:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:52436:5)
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:52370:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50271:5)
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
react-dom.development.js:13123 Uncaught Error: Objects are not valid as a React child (found: object with keys {claimid, emailbody}). If you meant to render a collection of children, use an array instead.
    at throwOnInvalidObjectType (react-dom.development.js:13123:1)
    at reconcileChildFibers (react-dom.development.js:14064:1)
    at reconcileChildren (react-dom.development.js:19193:1)
    at updateHostComponent (react-dom.development.js:19953:1)
    at beginWork (react-dom.development.js:21657:1)
    at beginWork$1 (react-dom.development.js:27465:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
    at workLoopSync (react-dom.development.js:26505:1)
    at renderRootSync (react-dom.development.js:26473:1)
    at recoverFromConcurrentError (react-dom.development.js:25889:1)
