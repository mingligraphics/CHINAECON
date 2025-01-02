<script>
    import data from "./data/Loss.json";
    import { tweened } from 'svelte/motion';
    import { scaleLinear } from "d3-scale";
    import { max } from "d3-array";
    import { interpolate } from "d3-interpolate";
    import { cubicOut } from "svelte/easing";
    import Steps from "/components/Steps.svelte";
  
    let initialData = data;
  
    // Group data by Code
    const groupedData = Array.from({ length: 6 }, (_, i) =>
      initialData.filter(d => d.Code === i + 1)
    );
  
    let currentStep = 0;
    let width = 450;
    let height = 550;
  
    const margin = {
      top: 10,
      right: 10,
      bottom: 20,
      left: 50
    };
  
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;
  
    // Define scales
    const yScale = scaleLinear()
      .domain([0, max(initialData, d => d.Value)])
      .range([innerHeight / 2, margin.top]);
  
    // Tweened animations for rectangles
    const firstRectHeight = tweened(0, {
      duration: 2000,
      easing: cubicOut
    });
  
    firstRectHeight.set(yScale(0) - yScale(18));
  
    // Tweened points for animations
    const tweenedPoints = tweened(groupedData[0], {
      delay: 0,
      duration: 800,
      easing: cubicOut,
      interpolate: (a, b) => interpolate(a, b)
    });
  
    $: {
      if (currentStep >= 0 && currentStep < groupedData.length) {
        tweenedPoints.set(groupedData[currentStep]);
      }
    }
  </script>
  
  <main>
    <section>
      <div class="sticky">
        <div class="chart-container" bind:clientWidth={width}>
          <svg {width} {height}>
            <g class="inner-chart" transform="translate({margin.left}, {margin.top})">
              <!-- First rectangle -->
              <rect
                x={innerWidth / 2 - 190}
                y={innerHeight / 2 - (yScale(0) - yScale(18)) / 2}
                width={280}
                height={$firstRectHeight}
                stroke="none"
                stroke-width={2}
                fill="#CE3139"
                opacity={currentStep > 0 ? "0.45" : "1"}
              />
  
              <!-- Additional rectangles -->
              {#each $tweenedPoints as d, i}
                <rect
                  x={innerWidth / 2 - 190}
                  y={innerHeight / 2 - (yScale(0) - yScale(18)) / 2}
                  width={280}
                  height={currentStep === 0 ? 0 : yScale(0) - yScale(d.Value)}
                  stroke={currentStep > 0 ? "black" : "none"}
                  stroke-width={currentStep > 0 ? "3" : "none"}
                  fill={currentStep > 0 ? "none" : "#CE3139"}
                  opacity="1"
                />
              {/each}
  
              <!-- Text elements -->
              {#each initialData as d, i}
                {#if currentStep == i}
                  <!-- Event name -->
                  <text
                    x={innerWidth / 2 - 51}
                    y={innerHeight / 2 - (yScale(0) - yScale(18)) / 2 - 15}
                    class="text-label"
                    font-weight="300"
                    font-size="17"
                    fill="#333333"
                    text-anchor="middle"
                  > 
                    {#if i === 0}
                    <tspan x={innerWidth / 2 - 54} dy="-1em">
                      {d.Event.split(' ').slice(0, 3).join(' ')}
                    </tspan>
                    <tspan x={innerWidth / 2 - 54} dy="1.2em">
                      {d.Event.split(' ').slice(3).join(' ')}
                    </tspan>
                    {:else if i === 3 || i === 4}
        <!-- No slicing for i === 3 or i === 4 -->
        {d.Event}
      {:else}
        <!-- Default slicing for other indices -->
        <tspan x={innerWidth / 2 - 48} dy="-1em">
          {d.Event.split(' ').slice(0, 5).join(' ')}
        </tspan>
        <tspan x={innerWidth / 2 - 48} dy="1.2em">
          {d.Event.split(' ').slice(5).join(' ')}
        </tspan>
      {/if}
                  
                  </text>
  
                  <!-- Value label -->
                  <text
                    x={innerWidth / 2 - 100}
                    y={innerHeight / 2 - (yScale(0) - yScale(18)) / 2 + 75}
                    class="number-label"
                    font-weight="500"
                    font-size="18"
                    fill={currentStep > 0 ? "#333333" : "white"}
                  >
                    ${currentStep == 0 ? 18 : d.Value} trillion
                  </text>
                {/if}
              {/each}
            </g>
          </svg>
        </div>
      </div>
      <Steps bind:currentStep />
    </section>
  </main>
  
  <style>
    .chart-container {
      height: 100%;
      width: 100%;
      max-width: 500px;
      display: flex;
      align-items: center;
    }
  
    .sticky {
      position: sticky;
      z-index: 1;
      height: 90vh;
      top: 5vh;
      margin-bottom: 1rem;
      display: flex;
      align-items: left;
      justify-content: left;
    }
  
    section {
      position: relative;
    }
  
    main {
      max-width: 1200px;
      margin: 0 auto;
    }
  
    .text-label {
      font-family: Retina, sans-serif;
    }
  
    .number-label {
      font-family: Retina, sans-serif;
    }
  </style>
  