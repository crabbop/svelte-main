<h1>Menu Design</h1>

<script>
  let numberOfButtons = 3; // Default number of buttons
  let selectedSideCodes = {
    runner: false,
    corp: false
  };
  let selectedTypeCodes = {
    agenda: false,
    asset: false,
    event: false,
    hardware: false,
    ice: false,
    identity: false,
    operation: false,
    program: false,
    resource: false,
    upgrade: false
  };
  let selectedFactionCodes = {
    jinteki: false,
    nbn: false,
    'haas-bioroid': false,
    'weyland-consortium': false,
    'neutral-corp': false,
    'neutral-runner': false,
    anarch: false,
    criminal: false,
    shaper: false
  };
</script>

<style>
  .options-container {
    display: flex;
    flex-direction: column;
    align-items: flex-start; /* Align items to the left */
    background-color: #a9a9a9; /* Darker grey background color */
    padding: 10px;
    border-radius: 8px;
    width: 250px; /* Reduce width */
    margin-right: 20px; /* Move to the left of the card image */
  }
  .options-container h3 {
    margin-bottom: 10px;
  }
  .options-container .option-group {
    display: flex;
    flex-direction: column;
    align-items: flex-start; /* Align items to the left */
    margin-bottom: 10px;
    width: 100%; /* Ensure full width */
  }
  .options-container .option-group label {
    margin-right: 10px;
  }
  .options-container .option-group input {
    margin-right: 5px;
  }
  .list-container {
    margin-top: 20px;
    padding: 10px;
    background-color: #d3d3d3; /* Light grey background color */
    border-radius: 8px;
  }
</style>

<div class="options-container">
  <h3>Options</h3>
  <div class="option-group">
    <label>Guess options:</label>
    <label><input type="radio" bind:group={numberOfButtons} value="2"> 2</label>
    <label><input type="radio" bind:group={numberOfButtons} value="3"> 3</label>
    <label><input type="radio" bind:group={numberOfButtons} value="4"> 4</label>
  </div>
  <div class="option-group">
    <label>Side:</label>
    <label><input type="checkbox" bind:checked={selectedSideCodes.runner}> Runner</label>
    <label><input type="checkbox" bind:checked={selectedSideCodes.corp}> Corp</label>
  </div>
  <div class="option-group">
    <label>Faction:</label>
    {#each Object.keys(selectedFactionCodes) as faction}
      <label><input type="checkbox" bind:checked={selectedFactionCodes[faction]}> 
        {#if faction === 'neutral-corp'}
          Neutral (Corp)
        {:else if faction === 'neutral-runner'}
          Neutral (Runner)
        {:else}
          {faction.charAt(0).toUpperCase() + faction.slice(1).replace('-', ' ')}
        {/if}
      </label>
    {/each}
  </div>
  <div class="option-group">
    <label>Type:</label>
    {#each Object.keys(selectedTypeCodes) as type}
      <label><input type="checkbox" bind:checked={selectedTypeCodes[type]}> {type.charAt(0).toUpperCase() + type.slice(1)}</label>
    {/each}
  </div>
</div>

<div class="list-container">
  <ul>
    <li>Guess options</li>
    <li>Side</li>
    <li>Faction</li>
    <li>Type</li>
  </ul>
</div>
