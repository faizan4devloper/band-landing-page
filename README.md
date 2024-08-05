Access to XMLHttpRequest at 'https://aiml-convai.s3.amazonaws.com/URLJson.json' from origin 'https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
App.js:89 Failed to fetch cards data AxiosError {message: 'Network Error', name: 'AxiosError', code: 'ERR_NETWORK', config: {…}, request: XMLHttpRequest, …}
fetchData @ App.js:89
await in fetchData (async)
(anonymous) @ App.js:93
commitHookEffectListMount @ react-dom.development.js:23150
commitPassiveMountOnFiber @ react-dom.development.js:24926
commitPassiveMountEffects_complete @ react-dom.development.js:24891
commitPassiveMountEffects_begin @ react-dom.development.js:24878
commitPassiveMountEffects @ react-dom.development.js:24866
flushPassiveEffectsImpl @ react-dom.development.js:27039
flushPassiveEffects @ react-dom.development.js:26984
(anonymous) @ react-dom.development.js:26769
workLoop @ scheduler.development.js:266
flushWork @ scheduler.development.js:239
performWorkUntilDeadline @ scheduler.development.js:533
Show 11 more frames
Show less
data.js:7 
        
        
       GET https://aiml-convai.s3.amazonaws.com/URLJson.json net::ERR_FAILED 403 (Forbidden)
dispatchXhrRequest @ xhr.js:195
xhr @ xhr.js:15
dispatchRequest @ dispatchRequest.js:51
_request @ Axios.js:173
request @ Axios.js:40
Axios.<computed> @ Axios.js:199
wrap @ bind.js:5
fetchURLJson @ data.js:7
getCardsData @ data.js:40
fetchData @ App.js:84
(anonymous) @ App.js:93
commitHookEffectListMount @ react-dom.development.js:23150
commitPassiveMountOnFiber @ react-dom.development.js:24926
commitPassiveMountEffects_complete @ react-dom.development.js:24891
commitPassiveMountEffects_begin @ react-dom.development.js:24878
commitPassiveMountEffects @ react-dom.development.js:24866
flushPassiveEffectsImpl @ react-dom.development.js:27039
flushPassiveEffects @ react-dom.development.js:26984
(anonymous) @ react-dom.development.js:26769
workLoop @ scheduler.development.js:266
flushWork @ scheduler.development.js:239
performWorkUntilDeadline @ scheduler.development.js:533
Show 18 more frames
Show less
manifest.json:1 
        
        
       GET https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/manifest.json 499
manifest.json:1 Manifest: Line: 1, column: 1, Syntax error.
a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/:1 Access to XMLHttpRequest at 'https://aiml-convai.s3.amazonaws.com/URLJson.json' from origin 'https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
App.js:89 Failed to fetch cards data AxiosError {message: 'Network Error', name: 'AxiosError', code: 'ERR_NETWORK', config: {…}, request: XMLHttpRequest, …}
fetchData @ App.js:89
await in fetchData (async)
(anonymous) @ App.js:93
commitHookEffectListMount @ react-dom.development.js:23150
invokePassiveEffectMountInDEV @ react-dom.development.js:25154
invokeEffectsInDev @ react-dom.development.js:27351
commitDoubleInvokeEffectsInDEV @ react-dom.development.js:27330
flushPassiveEffectsImpl @ react-dom.development.js:27056
flushPassiveEffects @ react-dom.development.js:26984
(anonymous) @ react-dom.development.js:26769
workLoop @ scheduler.development.js:266
flushWork @ scheduler.development.js:239
performWorkUntilDeadline @ scheduler.development.js:533
Show 10 more frames
Show less
data.js:7 
        
        
       GET https://aiml-convai.s3.amazonaws.com/URLJson.json net::ERR_FAILED 403 (Forbidden)
