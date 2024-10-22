ERROR in ./node_modules/axios/lib/adapters/http.js 8:0-24
Module not found: Error: Can't resolve 'http' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/adapters'
Did you mean './http'?
Requests that should resolve in the current directory need to start with './'.
Requests that start with a name are treated as module requests and resolve within module directories (node_modules, /home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules).
If changing the source code is not an option there is also a resolve options called 'preferRelative' which tries to resolve these kind of requests in the current directory too.

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "http": require.resolve("stream-http") }'
	- install 'stream-http'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "http": false }
ERROR in ./node_modules/axios/lib/adapters/http.js 9:0-26
Module not found: Error: Can't resolve 'https' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/adapters'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "https": require.resolve("https-browserify") }'
	- install 'https-browserify'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "https": false }
ERROR in ./node_modules/axios/lib/adapters/http.js 10:0-24
Module not found: Error: Can't resolve 'util' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/adapters'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
	- install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "util": false }
ERROR in ./node_modules/axios/lib/adapters/http.js 12:0-24
Module not found: Error: Can't resolve 'zlib' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/adapters'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "zlib": require.resolve("browserify-zlib") }'
	- install 'browserify-zlib'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "zlib": false }
ERROR in ./node_modules/axios/lib/adapters/http.js 19:0-28
Module not found: Error: Can't resolve 'stream' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/adapters'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "stream": require.resolve("stream-browserify") }'
	- install 'stream-browserify'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "stream": false }
ERROR in ./node_modules/axios/lib/helpers/AxiosTransformStream.js 3:0-28
Module not found: Error: Can't resolve 'stream' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/helpers'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "stream": require.resolve("stream-browserify") }'
	- install 'stream-browserify'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "stream": false }
ERROR in ./node_modules/axios/lib/helpers/ZlibHeaderTransformStream.js 3:0-28
Module not found: Error: Can't resolve 'stream' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/helpers'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "stream": require.resolve("stream-browserify") }'
	- install 'stream-browserify'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "stream": false }
ERROR in ./node_modules/axios/lib/helpers/formDataToStream.js 1:0-35
Module not found: Error: Can't resolve 'util' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/helpers'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
	- install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "util": false }
ERROR in ./node_modules/axios/lib/helpers/formDataToStream.js 2:0-34
Module not found: Error: Can't resolve 'stream' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/helpers'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "stream": require.resolve("stream-browserify") }'
	- install 'stream-browserify'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "stream": false }
ERROR in ./node_modules/axios/lib/platform/node/classes/URLSearchParams.js 3:0-22
Module not found: Error: Can't resolve 'url' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/axios/lib/platform/node/classes'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "url": require.resolve("url/") }'
	- install 'url'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "url": false }
ERROR in ./node_modules/follow-redirects/index.js 1:10-24
Module not found: Error: Can't resolve 'url' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/follow-redirects'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "url": require.resolve("url/") }'
	- install 'url'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "url": false }
ERROR in ./node_modules/follow-redirects/index.js 3:11-26
Module not found: Error: Can't resolve 'http' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/follow-redirects'
Did you mean './http'?
Requests that should resolve in the current directory need to start with './'.
Requests that start with a name are treated as module requests and resolve within module directories (node_modules, /home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules).
If changing the source code is not an option there is also a resolve options called 'preferRelative' which tries to resolve these kind of requests in the current directory too.

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "http": require.resolve("stream-http") }'
	- install 'stream-http'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "http": false }
ERROR in ./node_modules/follow-redirects/index.js 4:12-28
Module not found: Error: Can't resolve 'https' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/follow-redirects'
Did you mean './https'?
Requests that should resolve in the current directory need to start with './'.
Requests that start with a name are treated as module requests and resolve within module directories (node_modules, /home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules).
If changing the source code is not an option there is also a resolve options called 'preferRelative' which tries to resolve these kind of requests in the current directory too.

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "https": require.resolve("https-browserify") }'
	- install 'https-browserify'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "https": false }
ERROR in ./node_modules/follow-redirects/index.js 5:15-41
Module not found: Error: Can't resolve 'stream' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/follow-redirects'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "stream": require.resolve("stream-browserify") }'
	- install 'stream-browserify'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "stream": false }
ERROR in ./node_modules/follow-redirects/index.js 6:13-30
Module not found: Error: Can't resolve 'assert' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/follow-redirects'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "assert": require.resolve("assert/") }'
	- install 'assert'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "assert": false }
ERROR in ./node_modules/proxy-from-env/index.js 3:15-35
Module not found: Error: Can't resolve 'url' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/node_modules/proxy-from-env'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "url": require.resolve("url/") }'
	- install 'url'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "url": false }
