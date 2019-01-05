# Angular thoughts/notes/gotchas

* Components in `entryComponents` array are not tree shaked (this means non-entry components are tree shaked even if it is defined in the `declarations` array if not used). Components defined in Router, or defined in `bootstrap` array have the same effect as defining them in `entryComponents` array.  
* For services use;
 ```ts
 @Injectable({providedIn: 'root'})
 // or
 @Injectable({providedIn: SomeModule})
 ```
 As it will be tree shaked if unused.  Does this mean services defined in `providers` array are not tree shaked even if unused?  
* Lazy Loaded Module: 
    * Providers are scoped to a module where service is defined. i.e multiple injectors.
    * The router adds all of the providers from the root injector to the child injector. Hence root injector can be used with lazy loaded modules, but it follows the same `__proto__` pattern of JS, where it looks for local injector if it is there it will use that if not it will use the parent injector and so on. 
* Eagerly Loaded Module (default behaviour):
    * providers have global scope, i.e. one injector  
    * Component `providers` is an independent provider, only a component and its tree can see it. Each instance of the component has a new instance of the provider.  
* In both lazy loaded module and eagerly loaded module, if the same module is imported in the root module and feature module, it will be ok and should be imported in modules which need to use its declarations if only `declarations` is defined, however if `providers` is defined then we should have two instances of same providers, one in root module and one in feature module, that is why when making a module that defines `providers` we should have a way to include this providers or not.    
* The same module can be configured with `forRoot()` to make it suitable for loading it in root module. `forRoot` is a static method in the module definition that returns extra providers that should be included it were to be imported within the root module.
