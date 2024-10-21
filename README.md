






.tableCell {
  /* Default styles for the table cell */
  padding: 10px;
  border: 1px solid #ddd;
  transition: background-color 0.3s ease, box-shadow 0.3s ease, transform 0.3s ease;
}

.tableCell:hover {
  /* Hover effect styles */
  background-color: #e0e7ff; /* Subtle background color change on hover */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Adds a shadow to make the cell pop */
  transform: scale(1.05); /* Slight zoom effect on hover */
  cursor: pointer; /* Indicates interactivity */
}

.tableCell:hover .tableCellText {
  color: #4f46e5; /* Changes text color inside the cell on hover */
}

.tableCellText {
  /* Text-specific styles */
  font-weight: normal;
  transition: color 0.3s ease;
}useReduxContext.ts:17 Uncaught Error: could not find react-redux context value; please ensure the component is wrapped in a <Provider>
    at useReduxContext2 (useReduxContext.ts:17:1)
    at useSelector2 (useSelector.ts:175:1)
    at App (App.js:15:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at mountIndeterminateComponent (react-dom.development.js:20103:1)
    at beginWork (react-dom.development.js:21626:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27490:1)
useReduxContext2 @ useReduxContext.ts:17
useSelector2 @ useSelector.ts:175
App @ App.js:15
renderWithHooks @ react-dom.development.js:15486
mountIndeterminateComponent @ react-dom.development.js:20103
beginWork @ react-dom.development.js:21626
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
scheduleRoot @ react-dom.development.js:27849
(anonymous) @ react-refresh-runtime.development.js:284
performReactRefresh @ react-refresh-runtime.development.js:263
(anonymous) @ RefreshUtils.js:100
setTimeout
enqueueUpdate @ RefreshUtils.js:98
executeRuntime @ RefreshUtils.js:259
$ReactRefreshModuleRuntime$ @ LoginPage.js:77
./src/components/Login/LoginPage.js @ LoginPage.js:77
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
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:69
emit @ events.js:153
reloadApp @ reloadApp.js:38
warnings @ index.js:274
(anonymous) @ socket.js:62
client.onmessage @ WebSocketClient.js:45
Show 36 more frames
Show less
useReduxContext.ts:17 Uncaught Error: could not find react-redux context value; please ensure the component is wrapped in a <Provider>
    at useReduxContext2 (useReduxContext.ts:17:1)
    at useSelector2 (useSelector.ts:175:1)
    at App (App.js:15:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at mountIndeterminateComponent (react-dom.development.js:20103:1)
    at beginWork (react-dom.development.js:21626:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27490:1)
useReduxContext2 @ useReduxContext.ts:17
useSelector2 @ useSelector.ts:175
App @ App.js:15
renderWithHooks @ react-dom.development.js:15486
mountIndeterminateComponent @ react-dom.development.js:20103
beginWork @ react-dom.development.js:21626
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
scheduleRoot @ react-dom.development.js:27849
(anonymous) @ react-refresh-runtime.development.js:284
performReactRefresh @ react-refresh-runtime.development.js:263
(anonymous) @ RefreshUtils.js:100
setTimeout
enqueueUpdate @ RefreshUtils.js:98
executeRuntime @ RefreshUtils.js:259
$ReactRefreshModuleRuntime$ @ LoginPage.js:77
./src/components/Login/LoginPage.js @ LoginPage.js:77
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
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:69
emit @ events.js:153
reloadApp @ reloadApp.js:38
warnings @ index.js:274
(anonymous) @ socket.js:62
client.onmessage @ WebSocketClient.js:45
Show 37 more frames
Show less
LoginPage.js:77 The above error occurred in the <App> component:

    at App (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/main.ca31c904f282f7704968.hot-update.js:48:84)

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
scheduleRoot @ react-dom.development.js:27849
(anonymous) @ react-refresh-runtime.development.js:284
performReactRefresh @ react-refresh-runtime.development.js:263
(anonymous) @ RefreshUtils.js:100
setTimeout
enqueueUpdate @ RefreshUtils.js:98
executeRuntime @ RefreshUtils.js:259
$ReactRefreshModuleRuntime$ @ LoginPage.js:77
./src/components/Login/LoginPage.js @ LoginPage.js:77
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
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:69
emit @ events.js:153
reloadApp @ reloadApp.js:38
warnings @ index.js:274
(anonymous) @ socket.js:62
client.onmessage @ WebSocketClient.js:45
Show 34 more frames
Show less
react-refresh-runtime.development.js:315 Uncaught Error: could not find react-redux context value; please ensure the component is wrapped in a <Provider>
    at useReduxContext2 (useReduxContext.ts:17:1)
    at useSelector2 (useSelector.ts:175:1)
    at App (App.js:15:1)
    at renderWithHooks (react-dom.development.js:15486:1)
    at mountIndeterminateComponent (react-dom.development.js:20103:1)
    at beginWork (react-dom.development.js:21626:1)
    at beginWork$1 (react-dom.development.js:27465:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
    at workLoopSync (react-dom.development.js:26505:1)
    at renderRootSync (react-dom.development.js:26473:1)
useReduxContext2 @ useReduxContext.ts:17
useSelector2 @ useSelector.ts:175
App @ App.js:15
renderWithHooks @ react-dom.development.js:15486
mountIndeterminateComponent @ react-dom.development.js:20103
beginWork @ react-dom.development.js:21626
beginWork$1 @ react-dom.development.js:27465
performUnitOfWork @ react-dom.development.js:26596
workLoopSync @ react-dom.development.js:26505
renderRootSync @ react-dom.development.js:26473
recoverFromConcurrentError @ react-dom.development.js:25889
performSyncWorkOnRoot @ react-dom.development.js:26135
flushSyncCallbacks @ react-dom.development.js:12042
flushSync @ react-dom.development.js:26240
scheduleRoot @ react-dom.development.js:27849
(anonymous) @ react-refresh-runtime.development.js:284
performReactRefresh @ react-refresh-runtime.development.js:263
(anonymous) @ RefreshUtils.js:100
setTimeout
enqueueUpdate @ RefreshUtils.js:98
executeRuntime @ RefreshUtils.js:259
$ReactRefreshModuleRuntime$ @ LoginPage.js:77
./src/components/Login/LoginPage.js @ LoginPage.js:77
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
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:36
Promise.then
check @ dev-server.js:14
(anonymous) @ dev-server.js:69
emit @ events.js:153
reloadApp @ reloadApp.js:38
warnings @ index.js:274
(anonymous) @ socket.js:62
client.onmessage @ WebSocketClient.js:45
Show 34 more frames
Show less
