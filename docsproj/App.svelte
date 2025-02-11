<script>
  import { SCR_ROUTER_COMPONENT, SCR_CONFIG_STORE } from "../src/index.js";
  import SCR_Layout from "./components/SCR_Layout.svelte";
  import SCR_NotFound from "./components/SCR_NotFound.svelte";
  import SCR_Error from "./components/SCR_Error.svelte";
  import SCR_TestLoadingComponentWithBeforeEnter from "./components/pages/SCR_TestLoadingComponentWithBeforeEnter.svelte";

  // Setting configurations of the SCR Router
  // https://arthurgermano.github.io/svelte-client-router/#/svelte-client-router/configurationOptions
  SCR_CONFIG_STORE.setNotFoundRoute(
    "/svelte-client-router/myCustomNotFoundRoute"
  );
  SCR_CONFIG_STORE.setErrorRoute("/svelte-client-router/myCustomErrorRoute");
  SCR_CONFIG_STORE.setConsoleLogStores(false);
  SCR_CONFIG_STORE.setNavigationHistoryLimit(10);
  SCR_CONFIG_STORE.setHashMode(true);
  SCR_CONFIG_STORE.setUseScroll(true);
  SCR_CONFIG_STORE.setConsiderTrailingSlashOnMatchingRoute(true);
  SCR_CONFIG_STORE.setScrollProps({
    top: 0,
    left: 0,
    behavior: "smooth",
    timeout: 10,
  });

  // Setting global error function
  // https://arthurgermano.github.io/svelte-client-router/#/svelte-client-router/configurationOnError
  SCR_CONFIG_STORE.setOnError((err, routeObjParams) => {
    console.log("GLOBAL ERROR CONFIG", routeObjParams);
  });

  // Setting the route object definition
  // https://arthurgermano.github.io/svelte-client-router/#/svelte-client-router/routeObjectOptions
  let routes = [
    {
      name: "root",
      path: "/",
      beforeEnter: [
        (resolve, rFrom, rTo, params, payload) => {
          resolve({ redirect: "/svelte-client-router" });
        },
      ],
    },
    {
      name: "rootRoute",
      path: "/svelte-client-router",
      lazyLoadComponent: () => import("./components/pages/SCR_Presentation.svelte"),
      title: "SCR - Presentation",
    },
    {
      name: "installationRoute",
      path: "/svelte-client-router/installation",
      lazyLoadComponent: () => import("./components/pages/SCR_Installation.svelte"),
      title: "SCR - Installation",
    },
    {
      name: "gettingStartedRoute",
      path: "/svelte-client-router/gettingStarted",
      lazyLoadComponent: () => import("./components/pages/SCR_GettingStarted.svelte"),
      title: "SCR - Getting Started",
    },
    {
      name: "configurationOptionsRoute",
      path: "/svelte-client-router/configurationOptions",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_ConfigurationOptions.svelte"),
      title: "SCR - Configuration Options",
    },
    {
      name: "configurationGlobalBeforeEnterOptionRoute",
      path: "/svelte-client-router/configurationBeforeEnter",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_ConfigurationBeforeEnter.svelte"),
      title: "SCR - Configuration - Before Enter",
    },
    {
      name: "configurationOnErrorOptionRoute",
      path: "/svelte-client-router/configurationOnError",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_ConfigurationOnError.svelte"),
      title: "SCR - Configuration - On Error",
    },
    {
      name: "routeObjectOptionsRoute",
      path: "/svelte-client-router/routeObjectOptions",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_RouteObjectProperties.svelte"),
      title: "SCR - Route Object - Options",
    },
    {
      name: "routeObjectBeforeEnterRoute",
      path: "/svelte-client-router/routeObjectBeforeEnter",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_RouteObjectBeforeEnter.svelte"),
      title: "SCR - Route Object - Before Enter Functions",
    },
    {
      name: "routeObjectAfterBeforeEnterRoute",
      path: "/svelte-client-router/routeObjectAfterBeforeEnter",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_RouteObjectAfterBeforeEnter.svelte"),
      title: "SCR - Route Object - After Before Function",
    },
    {
      name: "routeObjectOnErrorRoute",
      path: "/svelte-client-router/routeObjectOnError",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_RouteObjectOnError.svelte"),
      title: "SCR - Route Object - On Error Function",
    },
    {
      name: "routeComponentPropertiesRoute",
      path: "/svelte-client-router/routeComponentProperties",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_RouteComponentProperties.svelte"),
      title: "SCR - Route Component - Properties",
    },
    {
      name: "routeComponentComponentsRoute",
      path: "/svelte-client-router/routeComponentComponents",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_RouteComponentComponents.svelte"),
      title: "SCR - Route Component - Components",
    },
    {
      name: "navigationRoutingRoute",
      path: "/svelte-client-router/navigationRouting",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_NavigationRouting.svelte"),
      title: "SCR - Navigation - Routing",
    },
    {
      name: "navigationStoreRoute",
      path: "/svelte-client-router/navigationStore",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_NavigationStore.svelte"),
      title: "SCR - Navigation - Store",
    },
    {
      name: "routerLinkPropertiesRoute",
      path: "/svelte-client-router/routerLinkProperties",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_RouterLinkProperties.svelte"),
      title: "SCR - Route Link - Properties",
    },
    {
      name: "routerStorePropertiesRoute",
      path: "/svelte-client-router/routerStoreProperties",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_RouterStoreProperties.svelte"),
      title: "SCR - Route Store - Properties",
    },
    {
      name: "testRegexPathRoute",
      path: "/svelte-client-router/:teste/testRegexPathParam",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_TestRegexPath.svelte"),
      title: "SCR - Test - Regex Path Route",
      forceReload: true
    },
    {
      name: "testRegexPath2Route",
      path: "/svelte-client-router/:firstParam/testRegexPathParam2/:secondParam",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_TestRegexPath2.svelte"),
      title: "SCR - Test - - Regex Path Route 2",
      forceReload: true
    },
    {
      name: "testLoadingComponentWithBeforeEnterRoute",
      path: "/svelte-client-router/testLoadingComponentWithBeforeEnter/:timeout",
      lazyLoadLoadingComponent: () =>
         import("./components/SCR_Loading.svelte"),
      component: SCR_TestLoadingComponentWithBeforeEnter,

      beforeEnter: (resolve, routeFrom, routeTo, routeObjParams, payload) => {
        setTimeout(() => resolve(true), routeObjParams?.pathParams?.timeout || 10)
      },
      title: "SCR - Test - Loading Component with Before Enter",
      forceReload: true
    },
    {
      name: "testAnyWildcardRoute",
      path: "/svelte-client-router/anyRouteWildcard/*/:somePathParam",
      lazyLoadComponent: () =>
        import("./components/pages/SCR_TestAnyRouteWildcard.svelte"),
      title: "SCR - Test - Any Route Wildcard",
      forceReload: true
    },
  ];
</script>

<!-- Using SCR_ROUTER_COMPONENT -->
<!-- https://arthurgermano.github.io/svelte-client-router/#/svelte-client-router/routeComponentProperties -->
<!-- https://arthurgermano.github.io/svelte-client-router/#/svelte-client-router/routeComponentComponents -->
<SCR_ROUTER_COMPONENT
  bind:routes
  defaultLayoutComponent={SCR_Layout}
  notFoundComponent={SCR_NotFound}
  errorComponent={SCR_Error}
/>
