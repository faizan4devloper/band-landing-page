Html Webpack Plugin:
  Error: Child compilation failed:
  Module not found: Error: Can't resolve '/home/ec2-user/environment/HCL-Web/web-app/node_modules/html-webpack-plugin/lib/loader.js' in '/home/ec2-user/environment/HCL-Web/web-app'  ModuleNotFoundError: Module not found: Error: Can't resolve '/home/ec2-user/environment/HCL-Web/web-app/node_modules/html-webpack-plugin/lib/loader.js' in '/home/ec2-user/environ  ment/HCL-Web/web-app'
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/webpack/lib/Compilation.js:2094:28
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/webpack/lib/NormalModuleFactory.js:895:13
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :10:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/webpack/lib/NormalModuleFactory.js:332:22
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :9:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/webpack/lib/NormalModuleFactory.js:509:22
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/webpack/lib/NormalModuleFactory.js:151:11
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/webpack/lib/NormalModuleFactory.js:729:23
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/neo-async/async.js:2830:7
      at done (/home/ec2-user/environment/HCL-Web/web-app/node_modules/neo-async/async.js:2925:13)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/webpack/lib/NormalModuleFactory.js:1165:23
      at finishWithoutResolve (/home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:567:11)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:656:15
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:714:5
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :16:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:714:5
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :27:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/DescriptionFilePlugin.js:89:43
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:714:5
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :15:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:714:5
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :15:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:714:5
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :16:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:714:5
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :27:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/DescriptionFilePlugin.js:89:43
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:714:5
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :16:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/Resolver.js:714:5
      at eval (eval at create (/home/ec2-user/environment/HCL-Web/web-app/node_modules/tapable/lib/HookCodeFactory.js:33:10), :15:1)
      at /home/ec2-user/environment/HCL-Web/web-app/node_modules/enhanced-resolve/lib/DirectoryExistsPlugin.js:41:15
      at process.processTicksAndRejections (node:internal/process/task_queues:81:21)
  
  - Compilation.js:2094 
    [web-app]/[webpack]/lib/Compilation.js:2094:28
  
  - NormalModuleFactory.js:895 
    [web-app]/[webpack]/lib/NormalModuleFactory.js:895:13
  
  
  - NormalModuleFactory.js:332 
    [web-app]/[webpack]/lib/NormalModuleFactory.js:332:22
  
  
  - NormalModuleFactory.js:509 
    [web-app]/[webpack]/lib/NormalModuleFactory.js:509:22
  
  - NormalModuleFactory.js:151 
    [web-app]/[webpack]/lib/NormalModuleFactory.js:151:11
  
  - NormalModuleFactory.js:729 
    [web-app]/[webpack]/lib/NormalModuleFactory.js:729:23
  
  - async.js:2830 
    [web-app]/[neo-async]/async.js:2830:7
  
  - async.js:2925 done
    [web-app]/[neo-async]/async.js:2925:13
  
  - NormalModuleFactory.js:1165 
    [web-app]/[webpack]/lib/NormalModuleFactory.js:1165:23
  
  - Resolver.js:567 finishWithoutResolve
    [web-app]/[enhanced-resolve]/lib/Resolver.js:567:11
  
  - Resolver.js:656 
    [web-app]/[enhanced-resolve]/lib/Resolver.js:656:15
  
  - Resolver.js:714 
    [web-app]/[enhanced-resolve]/lib/Resolver.js:714:5
  
  
  - Resolver.js:714 
    [web-app]/[enhanced-resolve]/lib/Resolver.js:714:5
  
  
  - DescriptionFilePlugin.js:89 
    [web-app]/[enhanced-resolve]/lib/DescriptionFilePlugin.js:89:43
  
  - Resolver.js:714 
    [web-app]/[enhanced-resolve]/lib/Resolver.js:714:5
  
  
  - Resolver.js:714 
    [web-app]/[enhanced-resolve]/lib/Resolver.js:714:5
  
  
  - Resolver.js:714 
    [web-app]/[enhanced-resolve]/lib/Resolver.js:714:5
  
  
  - Resolver.js:714 
    [web-app]/[enhanced-resolve]/lib/Resolver.js:714:5
  
  
  - DescriptionFilePlugin.js:89 
    [web-app]/[enhanced-resolve]/lib/DescriptionFilePlugin.js:89:43
  
  - Resolver.js:714 
    [web-app]/[enhanced-resolve]/lib/Resolver.js:714:5
  
  
  - Resolver.js:714 
    [web-app]/[enhanced-resolve]/lib/Resolver.js:714:5
  
  
  - DirectoryExistsPlugin.js:41 
    [web-app]/[enhanced-resolve]/lib/DirectoryExistsPlugin.js:41:15
  
  - task_queues:81 process.processTicksAndRejections
    node:internal/process/task_queues:81:21
  
  - child-compiler.js:174 
    [web-app]/[html-webpack-plugin]/lib/child-compiler.js:174:18
  
  - Compiler.js:605 finalCallback
    [web-app]/[webpack]/lib/Compiler.js:605:5
  
  - Compiler.js:640 
    [web-app]/[webpack]/lib/Compiler.js:640:11
  
  - Compiler.js:1329 
    [web-app]/[webpack]/lib/Compiler.js:1329:17
  
  
  - Hook.js:18 Hook.CALL_ASYNC_DELEGATE [as _callAsync]
    [web-app]/[tapable]/lib/Hook.js:18:14
  
  - Compiler.js:1325 
    [web-app]/[webpack]/lib/Compiler.js:1325:33
  
  - Compilation.js:2900 finalCallback
    [web-app]/[webpack]/lib/Compilation.js:2900:11
  
  - Compilation.js:3209 
    [web-app]/[webpack]/lib/Compilation.js:3209:11
  
  
  - Hook.js:18 Hook.CALL_ASYNC_DELEGATE [as _callAsync]
    [web-app]/[tapable]/lib/Hook.js:18:14
  
  - Compilation.js:3202 
    [web-app]/[webpack]/lib/Compilation.js:3202:38
  
  
  - Compilation.js:529 
    [web-app]/[webpack]/lib/Compilation.js:529:10
  
  - SourceMapDevToolPlugin.js:588 
    [web-app]/[webpack]/lib/SourceMapDevToolPlugin.js:588:10
  
  - async.js:2830 
    [web-app]/[neo-async]/async.js:2830:7
  
  - async.js:2857 Object.each
    [web-app]/[neo-async]/async.js:2857:9
  
  - SourceMapDevToolPlugin.js:412 
    [web-app]/[webpack]/lib/SourceMapDevToolPlugin.js:412:17
  
