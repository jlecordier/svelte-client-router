<script>
  import { SCR_ROUTER_LINK } from "../../../src/index.js";
  import SCR_PageFooter from "../SCR_PageFooter.svelte";
  import SCR_PushRouteButton from "../common/SCR_PushRouteButton.svelte";

  export let pathParams;
  let nextParam = "";
  let regex = /[A-Za-zÀ-ú0-9]/g;

  $: if (nextParam) {
    const match = nextParam.match(regex);
    const value = match ? match.join("").substr(0, 100) + "" : "";
    nextParam = value;
  }
</script>
<div class="scr-page">
  <h4 class="scr-h4">Test - Regex Path</h4>
  <p class="scr-text-justify">
    This route tests regex param path.
    <br />
    <br />
    The route param path passed is: <b>{pathParams.teste}</b>
    <br />
    <br />
    Try it!
  </p>
  <div class="mb-3">
    <label for="scr-next-param" class="form-label">Route Param</label>
    <input
      type="text"
      class="form-control form-control-sm"
      id="scr-next-param"
      placeholder=":teste"
      bind:value={nextParam}
    />
  </div>
  <SCR_ROUTER_LINK to={{ path: `/svelte-client-router/${nextParam}/testRegexPathParam` }}>
    <div class="scr-btn">Test Route With Param</div>
  </SCR_ROUTER_LINK>
  <hr class="scr-hr" />
  <center class="scr-center">
    <small class="scr-small">The configuration for this route.</small>
  </center>
  <pre
    class="scr-pre">
&#123;
  name: "testRegexPathRoute",
  path: "/svelte-client-router/:teste/testRegexPathParam",
  lazyLoadComponent: () =&gt
    import("./docs/pages/SCR_TestRegexPath.svelte"),
  title: "SCR - Test 1",
  forceReload: true
&#125;
</pre>
  <SCR_PageFooter>
    <div class="row">
      <div class="col">
        <SCR_PushRouteButton
          style="float:left"
          text="Previous"
          routeName="routerStorePropertiesRoute"
        />
        <SCR_PushRouteButton
          style="float:right;"
          text="Next"
          routeName="testRegexPath2Route"
        />
      </div>
    </div>
  </SCR_PageFooter>
</div>

<style>
  .scr-btn {
    border: 1px solid;
  }
</style>
