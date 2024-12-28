<script>
  export let currentStep;
  import Scrolly from "../helpers/Scrolly.svelte";

  $: steps = [
    `<p>China’s real estate crunch has vaporized trillions of dollars of household wealth tied up in property, equivalent to around $60,000 per household.</p>`,
    // 0 
    `<p>That is a larger loss than the fall in U.S. real estate from its pre-financial crisis peak to the beginning of a durable recovery some years later.</p>`,
  // 0 
  `<p>Include sinking stock markets and other assets, and the decline in U.S. household wealth during the crisis is still not as big as China’s current bust. In today’s dollars, U.S. households would have lost around $17 trillion. </p>`,
  `The wealth destroyed in China’s real estate bust is greater than the value of all listed stocks in China…`,
  // 2 
  `…and is roughly the same as the country’s entire economic output in a year. 
  <br><br><span style="color: #727272;font-weight: 300; font-family: Retina, sans-serif;font-size: 13px;line-height: 17px; margin-top:6px;">*As of October<br>Sources: Barclays (China's property sector); Federal Reserve Bank of St. Louis (U.S. household real estate and net worth); People's Bank of China via Macrobond (China's equity); IMF (China's GDP)</span>`
];

</script>
<div class="steps">
  <Scrolly bind:value={currentStep}>
    {#each steps as text, i}
    <div class="step" class:active={currentStep === i}>
      <div class="step-content {currentStep === steps.length - 1 ? 'last-step' : ''}">
        {@html text}
      </div>
    </div>
    {/each}
  </Scrolly>
</div>

<style>
  .step {
  height: 90vh;
  opacity: 0.3;
  transition: opacity 300ms ease;

/* vertically and horizontally center the steps */
  display: flex;
  justify-content: center;
  place-items: center;

}

.step-content {
position: relative;
box-sizing: border-box;
z-index: 1;
-webkit-transform: translate3d(0, 0, 0); /*for safari z-index issue*/
height: auto;
width: 100%;
max-width: 600px; 
margin: 0 auto;
font-family: RetinaLight, sans-serif;
font-size: 17px;
line-height: 1.5em;
font-weight: 400;
color: #333;
padding: 20px 25px 20px;
box-shadow: 0 0 1px 0 rgba(0, 0, 0, .4);
overflow: visible;
background-color: rgba(255, 255, 255, .9);
}

  /* Apply a different line height for the last step */
  .step-content.last-step {
    line-height: 1.2em; /* Adjust line height specifically for the last step */
  }

.step.active {
opacity: 1;
}

.steps {
position: relative;
z-index: 2;
pointer-events: none;
display: flex;
justify-content: right;
place-items: right;
}

/* mark{
padding-top: 3px;
padding-bottom: 3px;
padding-left: 4px;
padding-right: 4px;
margin-right: 1px;
margin-left: 1px;
border-radius: 4px;
white-space: nowrap;
font-weight: 500;
text-shadow: none;
} */
</style>