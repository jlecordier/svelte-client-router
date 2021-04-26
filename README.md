# Svelte Client Router (SCR)

## Svelte Client Router makes SPA routes easy!

Svelte Client Router is everything you need and think when routing SPA's. This is THE SVELTE ROUTER for SPA!

Designed to help make you in control of the routing of your Single Page Applications (SPA)!

## Index

- [Svelte Client Router (SCR)](#svelte-client-router-scr)
  - [Svelte Client Router makes SPA routes easy!](#svelte-client-router-makes-spa-routes-easy)
  - [Index](#index)
  - [Features](#features)
  - [Install](#install)
  - [Usage](#usage)
    - [SCR - Configuration Store](#scr---configuration-store)
    - [SCR - Route Object Definition](#scr---route-object-definition)
    - [SCR - Router Svelte Component](#scr---router-svelte-component)
    - [SCR - Navigation](#scr---navigation)
    - [SCR - RouterLink Component](#scr---routerlink-component)
    - [SCR - Router Store Component](#scr---router-store-component)

## Features

- Lazy Load Components
- Lazy Load Layouts
- Routes based in Stores
- Define Global Layout
- Ignore Global Layout or define it by Route
- Before Enter Global Functions
- Ignore Global Before Enter Function Per Route
- Before Enter Per Route Functions
- Execute Before Enter Router Functions before Global Before Enter Functions
- After Function (Before Enter Function Sequence Array)
- Route Title 
- Global Route Error Function
- Route Error Functions
- Loading Component on Route Changing
- Loading params for Loading Components per Route

## Install

To install Svelte Router on your svelte app:

with npm

```bash
npm i svelte-router-spa
```

## Usage

Ensure your local server is configured in SPA mode. 
In a default Svelte installation you need to edit your package.json and add _-s_ to `sirv public`.

```javascript
"start": "sirv public -s"
```

### SCR - Configuration Store

Configuration Store manages the behavior of the <abbr title="Svelte Client Router">SCR</abbr>.
Let's see the options we have here:

```javascript
{
  // ## Hash Mode checks route using #/ before the location path
  // ## for example http://localhost:5000/pathAAA#/pathBBB
  // ## it will consider only pathBBB and ignore pathAAA as path!
  // ## Boolean
  hashMode: false, // ## Default is false

  // ## Navigation History Limit is the amount of route history is added 
  // ## in the route navigation history list 
  // ## 0 or -1 equals to "no limit"
  // ## Integer
  navigationHistoryLimit: 10, // ## Default is 10

  // ## Save mode sets the type of saving history route and store
  // ## It can be set to one of this following values:
  // ## - localstorage: it saves route in the localstorage
  // ## - indexeddb: it saves route in the IndexedDb 
  // ## - none: Doesn't save anything - meaning when reload it starts fresh all values!
  // ## String
  saveMode: "localstorage", // ## Default is "localstorage"

  // ## Not Found Route Path
  // ## is the path that should redirect when not found a path in the application
  // ## String - must include "/"
  notFoundRoute: "/notFound", // ## Default is "/notFound"

  // ## Error Route Path
  // ## is the path that should redirect when an error occurs in the application
  // ## String - must include "/"
  errorRoute: "/error", // ## Default is "/error"

  // ## Console Log Error Messages logs in the console 
  // ## any error messages of the SCR for debugging purposes
  // ## Boolean
  consoleLogErrorMessages: true, // ## Default is true

  // ## Console Log Stores logs in the console 
  // ## any changes in the Router Store for debugging purposes
  // ## Boolean
  consoleLogStores: true, // ## Default is true

  // ## Uses Route Layout defines if you will be using layout 
  // ## for each route or not - can be ignored in the route 
  // ## Boolean
  usesRouteLayout: true, // ## Default is true

  // ## Before Enter defines a function or array of Functions
  // ## that must execute before each route
  // ## Function or Array - of Functions
  beforeEnter: undefined, // ## undefined

  // ## On Error defines a function when an error occurs
  // ## when routing
  // ## Function
  onError: undefined, // ## undefined
}

// ## EXAMPLE 
import { SCR_CONFIG_STORE } from "svelte-client-router";

SCR_CONFIG_STORE.setHashMode(false);
SCR_CONFIG_STORE.setNavigationHistoryLimit(10);
SCR_CONFIG_STORE.setSaveMode("localstorage");
SCR_CONFIG_STORE.setNotFoundRoute("/myCustomNotFoundRoute");
SCR_CONFIG_STORE.setErrorRoute("/myCustomErrorRoute");
SCR_CONFIG_STORE.setConsoleLogErrorMessages(true);
SCR_CONFIG_STORE.setConsoleLogStores(true);
SCR_CONFIG_STORE.setUsesRouteLayout(true);
SCR_CONFIG_STORE.setOnError((err, routeObjParams) => {
  console.log(routeObjParams)
  console.log("GLOBAL ERROR CONFIG", err);
});
SCR_CONFIG_STORE.setBeforeEnter([
  (resolve, routeFrom, routeTo, routeObjParams) => {
    console.log("Global Before Enter Route - 1");
    resolve(true);
  },
  (resolve, routeFrom, routeTo, routeObjParams) => {
    console.log("Global Before Enter Route - 2");
    resolve(true);
  },
]);

// ## Callback receives 2 params 
// ## 1) Error 
// ## 2) Parameters defined for the route, current route, from route, etc..
SCR_CONFIG_STORE.setOnError((err, routeObjParams) => {
  // ## Error Object
  // ## routeObjParams: 
  // ## {
  // ##   currentRoute:
  // ##   {
  // ##     hash: ""
  // ##     hostname: "localhost"
  // ##     name: "4"
  // ##     origin: "http://localhost:5000"
  // ##     params: {}
  // ##     pathname: "/test4"
  // ##     port: "5000"
  // ##     protocol: "http:"
  // ##   }
  // ##   fromRoute:
  // ##   {
  // ##     hash: ""
  // ##     hostname: "localhost"
  // ##     name: "4"
  // ##     origin: "http://localhost:5000"
  // ##     params: { myQueryParam: "OK", otherParam: "123" }
  // ##     pathname: "/test4"
  // ##     port: "5000"
  // ##     protocol: "http:"
  // ##   }
  // ##   {
  // ##   params:
  // ##     {
  // ##       myCustomParam: "OK THEN SHALL WE!"
  // ##     }
  // ##   }
  console.log("GLOBAL ERROR CONFIG", err);
});

// ## Receives a Function or an Array of Functions
// ## Callback receives 4 params
// ## 1) resolve function 
// ## Should resolve with: 
// ## - true - when everything went OK
// ## - false - when anything goes wrong
// ## - { redirect: "/some_route" }
// ## - { path: "/some_route" }
// ## - { name: "route_name" }
// ##
// ## 2) Route coming from
// ## 3) Route going to
// ## 4) Route defined params
SCR_CONFIG_STORE.setBeforeEnter([
  (resolve, routeFrom, routeTo, routeObjParams) => {
    // ## resolve - function
    // ## routeFrom: 
    // ## { 
    // ##    name: "Route Name 1",
    // ##    hash: "",
    // ##    hostname: "localhost",
    // ##    origin: "http://localhost:5000",
    // ##    params: {},
    // ##    pathname: "/test1",
    // ##    port: "5000",
    // ##    protocol: "http:",
    // ## }
    // ## routeTo: 
    // ## { 
    // ##    name: "Route Name 2",
    // ##    hash: "",
    // ##    hostname: "localhost",
    // ##    origin: "http://localhost:5000",
    // ##    params: { myQueryParam: "OK", otherParam: "123" },
    // ##    pathname: "/test2",
    // ##    port: "5000",
    // ##    protocol: "http:",
    // ## }
    // ## routeObjParams: 
    // ## { 
    // ##    params:
    // ##    {
    // ##      myCustomParams: "Route Defined Param!"
    // ##    }
    // ## }
    console.log("Global Before Enter Route - 1");
    resolve(true);
  },
  (resolve, routeFrom, routeTo, routeObjParams) => {
    console.log("Global Before Enter Route - 2");
    resolve(true);
  },
]);

// ## You can set the entire object with the following
SCR_CONFIG_STORE.setConfig({
  hashMode: false,
  navigationHistoryLimit: 10,
  saveMode: "localstorage",
  notFoundRoute: "/notFound",
  errorRoute: "/error",
  consoleLogErrorMessages: true,
  consoleLogStores: true,
  usesRouteLayout: true,
})
```

### SCR - Route Object Definition

Route Object is the definition of every route we have in the SPA application and is below our <abbr title="Svelte Client Router">SCR</abbr> Route Component.

Let's see the object format:

```javascript
import { SCR_ROUTER_COMPONENT } from "svelte-client-router";

import SCR_C1 from "./testComponents/SCR_C1.svelte";
import SCR_Layout from "./testComponents/SCR_C1.svelte";

{
  // ## Route Name
  // ## The name identifying this route
  // ## String - Obrigatory
  name: "routeName1",

  // ## Route Path
  // ## The path identifying this route
  // ## String - Obrigatory
  path: "/test1",

  // ## Component - the component that is going to be used 
  // ## for this route
  // ## Function - Imported component for this route
  component: SCR_C1,

  // ## Lazy Load Component - the component that must be loaded to be used 
  // ## for this route
  // ## Function - Function to load the component for this route
  lazyLoadComponent: () => import("./testComponents/SCR_C1.svelte"),

  // ## Lazy Load Layout Component - the layout component that must be loaded to be used 
  // ## for this route
  // ## Function - Function to load the layout component for this route
  lazyLoadLayoutComponent: () => import("./testComponents/SCR_Layout.svelte"),

  // ## Layout Component - the layout component that is going to be used 
  // ## for this route
  // ## Function - Imported layout component for this route
  layoutComponent: SRC_Layout,

  // ## Ignore Layout - if should ignore layout component
  // ## when you do not want to use global or local layout component
  // ## Boolean
  ignoreLayout: false,

  // ## Title - it defines the route title
  // ## String
  title: "First Route Title",

  // ## Params - all the params the should be available
  // for this route on any Before Enter Execution or 
  // After Before Enter Execution
  // ## Object
  params: {
    myCustomParam: "OK THEN SHALL WE!",
  },

  // ## Ignore Global Before Function - 
  // ## if should ignore defined global before function 
  // ## Boolean 
  ignoreGlobalBeforeFunction: false,

  // ## Execute Route Before Enter Function Before Global Before Function 
  // ## if should execute route before function sequence before 
  // ## global before enter execution
  // ## Boolean 
  executeRouteBEFBeforeGlobalBEF: false,

  // ## Loading Props - all props that must be available to
  // loading component when it is triggered
  loadingProps: { loadingText: "Carregando..." },

  // ## After Before Enter
  // ## a function to execute after enter the route
  // ## param:
  // ## {
  // ##   currentRoute: 
  // ##   { 
  // ##      name: "Route Name 1",
  // ##      hash: "",
  // ##      hostname: "localhost",
  // ##      origin: "http://localhost:5000",
  // ##      params: {},
  // ##      pathname: "/test1",
  // ##      port: "5000",
  // ##      protocol: "http:",
  // ##   }
  // ##   routeTo: 
  // ##   { 
  // ##      name: "Route Name 2",
  // ##      hash: "",
  // ##      hostname: "localhost",
  // ##      origin: "http://localhost:5000",
  // ##      params: { myQueryParam: "OK", otherParam: "123" },
  // ##      pathname: "/test2",
  // ##      port: "5000",
  // ##      protocol: "http:",
  // ##   }
  // ##   params:
  // ##   {
  // ##     myCustomParams: "Route Defined Param!"
  // ##   }
  // ## } 
  // ## Function 
  afterBeforeEnter: (param) => { console.log("AFTER ENTER", param);  },

  // ## Before Enter - a function or array of Functions
  // ## defining all functions that must be executed for this specific route
  // ## Function or Array (Functions)
  beforeEnter: [
    (resolve, routeFrom, routeTo, routeObjParams) => {
      // ## resolve - function
      // ## routeFrom: 
      // ## { 
      // ##    name: "Route Name 1",
      // ##    hash: "",
      // ##    hostname: "localhost",
      // ##    origin: "http://localhost:5000",
      // ##    params: {},
      // ##    pathname: "/test1",
      // ##    port: "5000",
      // ##    protocol: "http:",
      // ## }
      // ## routeTo: 
      // ## { 
      // ##    name: "Route Name 2",
      // ##    hash: "",
      // ##    hostname: "localhost",
      // ##    origin: "http://localhost:5000",
      // ##    params: { myQueryParam: "OK", otherParam: "123" },
      // ##    pathname: "/test2",
      // ##    port: "5000",
      // ##    protocol: "http:",
      // ## }
      // ## routeObjParams: 
      // ## { 
      // ##    params:
      // ##    {
      // ##      myCustomParams: "Route Defined Param!"
      // ##    }
      // ## }
      setTimeout(() => resolve(true), 2000);
      console.log("beforeEnter Executed");
      resolve(true);
    },
    (resolve, routeFrom, routeTo, routeObjParams) => {
      setTimeout(() => resolve(true), 1000);
      console.log("beforeEnter Executed2");
      resolve({ redirect: "/test2" });
    },
  ],

  // ## On Error - a function to execute when somenthing goes wrong on loading
  // ## this specific route
  // ## Error Object
  // ## routeObjParams: 
  // ## {
  // ##   currentRoute:
  // ##   {
  // ##     hash: ""
  // ##     hostname: "localhost"
  // ##     name: "4"
  // ##     origin: "http://localhost:5000"
  // ##     params: {}
  // ##     pathname: "/test4"
  // ##     port: "5000"
  // ##     protocol: "http:"
  // ##   }
  // ##   fromRoute:
  // ##   {
  // ##     hash: ""
  // ##     hostname: "localhost"
  // ##     name: "4"
  // ##     origin: "http://localhost:5000"
  // ##     params: { myQueryParam: "OK", otherParam: "123" }
  // ##     pathname: "/test4"
  // ##     port: "5000"
  // ##     protocol: "http:"
  // ##   }
  // ##   {
  // ##   params:
  // ##     {
  // ##       myCustomParam: "OK THEN SHALL WE!"
  // ##     }
  // ##   }
  // ## Function
  onError: (err, params) => {
    console.log("ERROR DEFINED ROUTER C1", err);
    console.log(params)
  },
},

<SCR_ROUTER_COMPONENT bind:routes />

```

### SCR - Router Svelte Component

Route Svelte Component will control the selection of the route, execute all the logic and return accordingly you 
specified in the router object. 

Let's see this component properties and possibilities:

```javascript
import { SCR_ROUTER_COMPONENT } from "svelte-client-router";

// ## Your route definitions as exampled above
const routes = [
  ...
]

// ## THE SVELTE CLIENT ROUTER COMPONENT!
// ## Properties
// ## the route object definition
// ## Array of routes
export let routes;

// ## The Not Found Component - SCR has a Not Found Component by Default - 
// ## If you not specify it will use the default one
// ## It is used when a not defined route is entered
// ## Function
export let notFoundComponent = SCR_NotFound;

// ## The Error Component - SCR has a Error Component by Default - 
// ## If you not specify it will use the default one
// ## It is used when an error has occured
// ## Function
export let errorComponent = SCR_Error;

// ## The Layout Component - SCR has a Layout Component by Default - 
// ## If you not specify it will use the default one
// ## It is used when layout is not ignoring Layout
// ## Function
export let defaultLayoutComponent = SCR_Layout;

// ## The Loading Component - SCR has a Loading Component by Default - 
// ## If you not specify it will use the default one
// ## It is used when it is executing Before Enter Route or Global Before Enter
// ## Function
export let loadingComponent = SCR_Loading;

// ## AllProps - all the properties that must be passed to all Routes and components
// ## Object - default is an empty object
export let allProps = {};

// ## All Loading Props - all the properties that must be passed to all Routes when loading
// ## Object - default is an empty object
export let allLoadingProps = {};

<SCR_ROUTER_COMPONENT 
  bind:routes 
  notFoundComponent 
  errorComponent 
  defaultLayoutComponent 
  loadingComponent
  allProps
  allLoadingProps
/>

```

### SCR - Navigation

<abbr title="Svelte Client Router">SCR</abbr> navigation can be very simple. You can import the navigation store or
just the methods to navigate.

Let's see them:

```javascript
// < -- You Svelte Component Definition-- >
<script>
    import { pushRoute, backRoute, SCR_NAVIGATE_STORE } from "svelte-client-router";
</script>

<main>
  My Svelte Component
  <br>
  <button on:click={() => { pushRoute({ name: 'routeNameOne'}); }}>
    Go To Route Named: routeNameOne
  </button>
  <br>
  <button on:click={() => { pushRoute({ path: '/routeNameOne'}); }}>
    Go To Route Path: /routeNameOne
  </button>
  <br>
  <button on:click={() => { SCR_NAVIGATE_STORE.pushRoute({ name: 'routeNameTwo'}); }}>
    Go To Route Named: routeNameTwo
  </button>
  <br>
  <button on:click={() => { SCR_NAVIGATE_STORE.pushRoute({ path: 'routeNameTwo'}); }}>
    Go To Route Path: routeNameTwo
  </button>
  <br>
  <button on:click={() => { backRoute(); }}>
    Back to Previous Route
  </button>
  <br>
  <button on:click={() => { SCR_NAVIGATE_STORE.backRoute(); }}>
    Back to Previous Route
  </button>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }
</style>

```

The assinature of the methods

```javascript

// ## Push Route Function 
// ## -----------------------------------------------------------------------------------
// ## -- First Argument is where to send accepts the following:
// ## String /pathToRoute
// ## Object { path: "/pathToRoute" }
// ## Object { name: "myRouteName" }
// ## -----------------------------------------------------------------------------------
// ## -- Second Argument custom params to pass to route they will be considered
// ## and made available on before functions after enter route function and component
// ## Object { someCustomParams: { isAvailable: "Hell ya" }}
// ## -----------------------------------------------------------------------------------
// ## -- Third Argument a custom onError to execute if something goes wrong.
// ## it will override, ONLY WHEN SET, the route on error defined in the routes object
// ## -----------------------------------------------------------------------------------
// ## Function
pushRoute(to, props, onError);

// ## Back Route Function - returns last route definition
// ## Function
backRoute()

```

### SCR - RouterLink Component

<abbr title="Svelte Client Router">SCR</abbr> RouterLink Component is a component to make easy clickable go to route.

Let's see it:

```javascript
// < -- You Svelte Component Definition-- >
<script>
  import { SCR_ROUTER_LINK } from "svelte-client-router";
</script>

<main>
  <SCR_ROUTER_LINK 
  to={{name: "myRouteNameThree" }}
  props={{ pushCustomParam: "someCustomParams" }}
  elementProps={{ style:"background-color: green" }}
  onError={() => console.log("Execute this instead error defined on router object! - Only if something goes wrong")}
  >
    <button>Click to Go to Defined Route Named: myRouteNameThree!</button>
  </SCR_ROUTER_LINK>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }
</style>

```

### SCR - Router Store Component

<abbr title="Svelte Client Router">SCR</abbr> Router Store Component is the store where all the route definitions
are updated and controlled. You can check real time what is happening.

Let's see it:

```javascript
// < -- You Svelte Component Definition-- >
<script>
  import { SCR_ROUTER_COMPONENT } from "svelte-client-router";
</script>

{
  // ## Routes
  // ## The Array object defined on initialization of the application
  // ## Array
  routes: [],

  // ## Current Location
  // ## Used inside the component to identify where it is coming from
  // ## updating based on the configuration store
  // ## Object
  currentLocation: undefined,

  // ## Current Route
  // ## The object with current route information
  // ## Updated before "after before enter function" execution
  // ## Object
  currentRoute: {
    name: undefined,
    pathname: undefined,
    params: [],
    hostname: undefined,
    protocol: undefined,
    port: undefined,
    origin: undefined,
    hash: undefined,
  },

  // ## From Route
  // ## The object with from route information
  // ## Updated before "after before enter function" execution
  // ## Object
  fromRoute: {
    name: undefined,
    pathname: undefined,
    params: [],
    hostname: undefined,
    protocol: undefined,
    port: undefined,
    origin: undefined,
    hash: undefined,
  },

  // ## Navigation History
  // ## An array with all the route history objects - the limit is 
  // ## defined in the configuration store
  // ## Updated before "after before enter function" execution
  // ## Array
  navigationHistory: [],
}

// ## Methods available on the STORE OBJECTS
// ## -----------------------------------------------------------------------------------
// ## --- ATTENTION Only use methods to search and identify routes - DO NOT SET ANYTHING
// ## --- This is because the component use this store as base to control things !!!!!!!
// ## -----------------------------------------------------------------------------------
// ## setRoutes,
// ## getRoutes,
// ## setCurrentRoute,
// ## getCurrentRoute,
// ## setFromRoute,
// ## getFromRoute,
// ## setNavigationHistory,
// ## getNavigationHistory,
// ## pushNavigationHistory,
// ## popNavigationHistory,
// ## setCurrentLocation,
// ## getCurrentLocation,
// ## getConfig,

```