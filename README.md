Uncaught TypeError: emailBody.split is not a function
    at formatEmailBody (GenerateEmail.js:38:1)
    at GenerateEmail (GenerateEmail.js:72:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at updateFunctionComponent (react-dom.development.js:19617:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27490:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
formatEmailBody @ GenerateEmail.js:38
GenerateEmail @ GenerateEmail.js:72
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
performSyncWorkOnRoot @ react-dom.development.js:26124
flushSyncCallbacks @ react-dom.development.js:12042
flushSync @ react-dom.development.js:26240
scheduleRefresh @ react-dom.development.js:27834
(anonymous) @ react-refresh-runtime.development.js:304
performReactRefresh @ react-refresh-runtime.development.js:293
(anonymous) @ RefreshUtils.js:100
setTimeout
enqueueUpdate @ RefreshUtils.js:98
executeRuntime @ RefreshUtils.js:259
$ReactRefreshModuleRuntime$ @ GenerateEmail.js:87
./src/components/ManageClaims/GenerateEmail.js @ GenerateEmail.js:87
options.factory @ react refresh:6
__webpack_require__ @ bootstrap:22
_requireSelf @ hot module replacement:101
apply @ jsonp chunk loading:404
(anonymous) @ hot module replacement:341
internalApply @ hot module replacement:339
(anonymous) @ hot module replacement:277
waitForBlockingPromises @ hot module replacement:232
(anonymous) @ hot module replacement:275
Promise.then
(anonymous) @ hot module replacement:274
Promise.then
(anonymous) @ hot module replacement:255
Promise.then
hotCheck @ hot module replacement:246
check @ dev-server.js:14
(anonymous) @ dev-server.js:69
emit @ events.js:153
reloadApp @ reloadApp.js:38
ok @ index.js:227
(anonymous) @ socket.js:62
client.onmessage @ WebSocketClient.js:45Understand this errorAI
GenerateEmail.js:38 Uncaught TypeError: emailBody.split is not a function
    at formatEmailBody (GenerateEmail.js:38:1)
    at GenerateEmail (GenerateEmail.js:72:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at updateFunctionComponent (react-dom.development.js:19617:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27490:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
formatEmailBody @ GenerateEmail.js:38
GenerateEmail @ GenerateEmail.js:72
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
performSyncWorkOnRoot @ react-dom.development.js:26135
flushSyncCallbacks @ react-dom.development.js:12042
flushSync @ react-dom.development.js:26240
scheduleRefresh @ react-dom.development.js:27834
(anonymous) @ react-refresh-runtime.development.js:304
performReactRefresh @ react-refresh-runtime.development.js:293
(anonymous) @ RefreshUtils.js:100
setTimeout
enqueueUpdate @ RefreshUtils.js:98
executeRuntime @ RefreshUtils.js:259
$ReactRefreshModuleRuntime$ @ GenerateEmail.js:87
./src/components/ManageClaims/GenerateEmail.js @ GenerateEmail.js:87
options.factory @ react refresh:6
__webpack_require__ @ bootstrap:22
_requireSelf @ hot module replacement:101
apply @ jsonp chunk loading:404
(anonymous) @ hot module replacement:341
internalApply @ hot module replacement:339
(anonymous) @ hot module replacement:277
waitForBlockingPromises @ hot module replacement:232
(anonymous) @ hot module replacement:275
Promise.then
(anonymous) @ hot module replacement:274
Promise.then
(anonymous) @ hot module replacement:255
Promise.then
hotCheck @ hot module replacement:246
check @ dev-server.js:14
(anonymous) @ dev-server.js:69
emit @ events.js:153
reloadApp @ reloadApp.js:38
ok @ index.js:227
(anonymous) @ socket.js:62
client.onmessage @ WebSocketClient.js:45Understand this errorAI
GenerateEmail.js:87 The above error occurred in the <GenerateEmail> component:

    at GenerateEmail (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/main.47fbd9c76384b371500e.hot-update.js:30:80)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:51697:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:52431:5)
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:52365:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50266:5)
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
performSyncWorkOnRoot @ react-dom.development.js:26156
flushSyncCallbacks @ react-dom.development.js:12042
flushSync @ react-dom.development.js:26240
scheduleRefresh @ react-dom.development.js:27834
(anonymous) @ react-refresh-runtime.development.js:304
performReactRefresh @ react-refresh-runtime.development.js:293
(anonymous) @ RefreshUtils.js:100
setTimeout
enqueueUpdate @ RefreshUtils.js:98
executeRuntime @ RefreshUtils.js:259
$ReactRefreshModuleRuntime$ @ GenerateEmail.js:87
./src/components/ManageClaims/GenerateEmail.js @ GenerateEmail.js:87
options.factory @ react refresh:6
__webpack_require__ @ bootstrap:22
_requireSelf @ hot module replacement:101
apply @ jsonp chunk loading:404
(anonymous) @ hot module replacement:341
internalApply @ hot module replacement:339
(anonymous) @ hot module replacement:277
waitForBlockingPromises @ hot module replacement:232
(anonymous) @ hot module replacement:275
Promise.then
(anonymous) @ hot module replacement:274
Promise.then
(anonymous) @ hot module replacement:255
Promise.then
hotCheck @ hot module replacement:246
check @ dev-server.js:14
(anonymous) @ dev-server.js:69
emit @ events.js:153
reloadApp @ reloadApp.js:38
ok @ index.js:227
(anonymous) @ socket.js:62
client.onmessage @ WebSocketClient.js:45Understand this errorAI
react-refresh-runtime.development.js:315 Uncaught TypeError: emailBody.split is not a function
    at formatEmailBody (GenerateEmail.js:38:1)
    at GenerateEmail (GenerateEmail.js:72:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at updateFunctionComponent (react-dom.development.js:19617:1)
    at beginWork (react-dom.development.js:21640:1)
    at beginWork$1 (react-dom.development.js:27465:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
    at workLoopSync (react-dom.development.js:26505:1)
    at renderRootSync (react-dom.development.js:26473:1)
    at recoverFromConcurrentError (react-dom.development.js:25889:1)
