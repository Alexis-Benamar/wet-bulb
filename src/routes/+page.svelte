<script>
  let temperature = $state(20);
  let humidity = $state(60);
  let incorrectValue = $derived(!temperature || !humidity);

  let result = $derived(
    temperature * Math.atan(0.151977 * Math.sqrt(humidity + 8.313659))
    + Math.atan(temperature + humidity)
    - Math.atan(humidity - 1.676331)
    + 0.00391838 * Math.pow(humidity, 3/2) * Math.atan(0.023101 * humidity)
    - 4.686035
  );
  let resultFixed = $derived(result.toFixed(1));

  $inspect(temperature, humidity, result);
</script>

<svelte:head>
	<title>Wet Bulb Calculator</title>
	<meta name="description" content="Calculator for wet bulb temperature" />
</svelte:head>

<main>
  <hgroup>
    <h1>Wet bulb temperature calculator</h1>
    <p><i>It's getting hot in here</i></p>
  </hgroup>
  <section>
    <h2>What is it ?</h2>
    <p>Wet bulb temperature is a mesure of ambiant temperature that <strong>takes into account the humidity level.</strong></p>
    <p>With climate change, we get more and more events where temperature & humidity are so high that we lose the ability to cool ourselves through sweating. Meaning, people simply <strong>die of heat. Regardless of physical condition.</strong> Knowing the <em>wet bulb temperature</em> becomes increasingly important, to prevent accidents.</p>
    <a href="#results-interpretation">how to interpret results ?</a>
  </section>
  <section class="calculator">
    <h2>Calculator</h2>
    <small><i>This calculator uses <a target="_blank" href="https://journals.ametsoc.org/view/journals/apme/50/11/jamc-d-11-0143.1.xml">Stull's formula</a></i></small>
    <div class="temp-form">
      <label>
        temperature (°C)
        <input type="number" bind:value={temperature} step="0.01" min="-100" max="100"/>
      </label>
      <label>
        humidity (%)
        <input type="number" bind:value={humidity} step="0.01" min="0" max="100" />
      </label>
    </div>
    <div class="form-output">
      <p><b>Result</b></p>
      <output class:warning={result >= 31}>{incorrectValue ? '-' : resultFixed}</output> °C
    </div>
    {#if result >= 31}
      <p class="warning-text">This temperature is above the accepted threshold where it starts having direct consequences on body temperature regulation, with serious danger.</p>
    {/if}
  </section>
  <section id="results-interpretation">
    <h2>Understanding the result</h2>
    <h3>Value</h3>
    <p>First of all, the wet bulb temperature is always below or equal to the regular air temperature. For example, at <b>20°C</b> with a humidity level of <b>60%</b>, the wet bulb temperature is <b>15°C</b>. This is caused by the water evaporation taking energy from the ambiant air. Though it can only take on so much water, until it reaches 100% humidity, where it gets fully saturated.</p>
    <h3>When is it dangerous</h3>
    <p>Wet bulb temperature between <em>31°C and 35°C</em> are lethally dangerous in the short term, even for young, healthy adults.</p>
    <ul>
      <li>2010 study : <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC2906879/" target="_blank" rel="noopener noreferrer">https://pmc.ncbi.nlm.nih.gov/articles/PMC2906879/</a></li>
      <li>2021 study : <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC8799385/" target="_blank" rel="noopener noreferrer">https://pmc.ncbi.nlm.nih.gov/articles/PMC8799385/</a></li>
    </ul>
  </section>
</main>
<footer>
  <i>made in 2025 with sweat and tears (mostly sweat)</i>
</footer>

<style>
  hgroup {
    border: none;
    text-align: center;
    margin-block: 2rem;

    h1 {
      border: none;
    }
  }

  .calculator {
    display: grid;
    gap: 1rem;
    grid-template-columns: 2fr 1fr;

    h2, small {
      grid-column: span 2;
    }

    .temp-form {
      display: flex;
      gap: 2rem;
    }

    .form-output {
      p {
        margin-bottom: 0;
      }

      output {
        display: inline-block;
        min-height: 1.5rem;
        padding: .25rem .5rem;
        margin-top: .25rem;

        &.warning {
          color: red;
          border-color: red;
        }
      }
    }

    .temp-form,
    .form-output {
      font-size: 1.5rem;
    }

    .warning-text {
      grid-column: span 2;
      text-wrap: pretty;
      color: red;
    }
  }

  footer {
    margin-top: 3rem;
    padding: 2rem;
  }
</style>

