<script>
  let numberOfButtons = 3; // Default number of buttons
  const sliderWidth = 180; // Slider width
  let selectedSideCodes = {
    runner: 'false',
    corp: 'false'
  };
  let selectedTypeCodes = {
    agenda: 'false',
    asset: 'false',
    event: 'false',
    hardware: 'false',
    ice: 'false',
    identity: 'false',
    operation: 'false',
    program: 'false',
    resource: 'false',
    upgrade: 'false'
  };
  let selectedFactionCodes = {
    jinteki: 'false',
    nbn: 'false',
    'haas-bioroid': 'false',
    'weyland-consortium': 'false',
    'neutral-corp': 'false',
    'neutral-runner': 'false',
    anarch: 'false',
    criminal: 'false',
    shaper: 'false'
  };
  let selectedPackCodes = {
    'rebellion-without-rehearsal': 'false',
    'the-automata-initiative': 'false',
    'system-update-2021': 'false',
    'system-gateway': 'false'
  };

  let isCollapsed = false;

  function toggleSelection(type, key) {
    console.log(`Toggling ${key} from ${type[key]}`);
    const newType = { ...type };
    if (newType[key] === 'false') {
      newType[key] = 'include';
    } else if (newType[key] === 'include') {
      newType[key] = 'exclude';
    } else {
      newType[key] = 'false';
    }
    console.log(`Toggled ${key} to ${newType[key]}`);
    if (type === selectedSideCodes) {
      selectedSideCodes = newType;
    } else if (type === selectedTypeCodes) {
      selectedTypeCodes = newType;
    } else if (type === selectedFactionCodes) {
      selectedFactionCodes = newType;
    } else if (type === selectedPackCodes) {
      selectedPackCodes = newType;
    }
  }

  function displayValue(value) {
    if (value === 'include') return '✓';
    if (value === 'exclude') return '✗';
    return '';
  }

  function toggleCollapse() {
    isCollapsed = !isCollapsed;
  }
</script>

<style>
  .options-container {
    display: flex;
    flex-direction: column;
    align-items: flex-start; /* Align items to the left */
    background-color: #a9a9a9; /* Darker grey background color */
    padding: 10px;
    border-radius: 8px;
    width: 205px; /* Adjust width to ensure 10px padding on both sides of the buttons */
    margin-right: 20px; /* Move to the left of the card image */
    transition: transform 0.3s ease;
    transform: translateX(var(--translateX, 0));
    position: relative;
    font-size: 0.9em; /* Make font size smaller */
  }
  .options-container.collapsed {
    --translateX: -96%; /* Collapse to 95% */
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
  .options-container .option-group button {
    margin-right: 5px;
    padding: 2px 5px; /* Reduced padding */
    border: none;
    border-radius: 4px;
    cursor: pointer;
    width: 185px; /* Set button width to 185px */
    display: flex;
    justify-content: space-between; /* Align text and symbol */
  }
  .state-container {
    margin-top: 20px;
    padding: 10px;
    background-color: #d3d3d3; /* Light grey background color */
    border-radius: 8px;
    font-size: 0.9em; /* Make font size smaller */
  }
  .include {
    background-color: green;
    color: white;
  }
  .exclude {
    background-color: rgb(185, 23, 23);
    color: white;
  }
  .false {
    background-color: grey;
    color: white;
  }
  .toggle-button {
    position: absolute;
    top: 10px;
    right: var(--button-right, 45px); /* Adjust position based on text length */
    padding: 5px 10px;
    background-color: #333;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    z-index: 1000; /* Ensure the button is on top */
  }
</style>

<div class="options-container {isCollapsed ? 'collapsed' : ''}">
  <button class="toggle-button" on:click={toggleCollapse} style="--button-right: {isCollapsed ? '-110px' : '45px'};">
    {isCollapsed ? 'Filter Options' : 'Hide'}
  </button>
  <h3><b>Filter Options</b></h3>
  <div class="option-group">
    <label>Side:</label>
    {#each Object.keys(selectedSideCodes) as side}
      <button class={selectedSideCodes[side]} on:click={() => toggleSelection(selectedSideCodes, side)}>
        <span>{side.charAt(0).toUpperCase() + side.slice(1)}</span>
        <span>{displayValue(selectedSideCodes[side])}</span>
      </button>
    {/each}
  </div>
  <div class="option-group">
    <label>Faction:</label>
    {#each Object.keys(selectedFactionCodes) as faction}
      <button class={selectedFactionCodes[faction]} on:click={() => toggleSelection(selectedFactionCodes, faction)}>
        <span>
          {#if faction === 'neutral-corp'}
            Neutral (Corp)
          {:else if faction === 'neutral-runner'}
            Neutral (Runner)
          {:else}
            {faction.charAt(0).toUpperCase() + faction.slice(1).replace('-', ' ')}
          {/if}
        </span>
        <span>{displayValue(selectedFactionCodes[faction])}</span>
      </button>
    {/each}
  </div>
  <div class="option-group">
    <label>Type:</label>
    {#each Object.keys(selectedTypeCodes) as type}
      <button class={selectedTypeCodes[type]} on:click={() => toggleSelection(selectedTypeCodes, type)}>
        <span>{type.charAt(0).toUpperCase() + type.slice(1)}</span>
        <span>{displayValue(selectedTypeCodes[type])}</span>
      </button>
    {/each}
  </div>
  <div class="option-group">
    <label>Startup Pack:</label>
    {#each Object.keys(selectedPackCodes) as pack}
      <button class={selectedPackCodes[pack]} on:click={() => toggleSelection(selectedPackCodes, pack)}>
        <span>{pack.split('-').map(word => word.charAt(0).toUpperCase() + word.slice(1)).join(' ')}</span>
        <span>{displayValue(selectedPackCodes[pack])}</span>
      </button>
    {/each}
  </div>
  <div class="option-group">
    <label>Guess options:</label>
    <input type="range" min="2" max="4" bind:value={numberOfButtons} style="width: {sliderWidth}px;">
    <div>Selected: {numberOfButtons}</div>
  </div>
</div>

<div class="state-container">
  <h3><b>State</b></h3>
  <p><b>Number of Buttons:</b> {numberOfButtons}</p>
  <p><b>Side Codes:</b></p>
  <ul>
    {#each Object.keys(selectedSideCodes) as side}
      <li>{side.charAt(0).toUpperCase() + side.slice(1)}: {displayValue(selectedSideCodes[side])}</li>
    {/each}
  </ul>
  <p><b>Faction Codes:</b></p>
  <ul>
    {#each Object.keys(selectedFactionCodes) as faction}
      <li>{faction.charAt(0).toUpperCase() + faction.slice(1).replace('-', ' ')}: {displayValue(selectedFactionCodes[faction])}</li>
    {/each}
  </ul>
  <p><b>Type Codes:</b></p>
  <ul>
    {#each Object.keys(selectedTypeCodes) as type}
      <li>{type.charAt(0).toUpperCase() + type.slice(1)}: {displayValue(selectedTypeCodes[type])}</li>
    {/each}
  </ul>
  <p><b>Pack Codes:</b></p>
  <ul>
    {#each Object.keys(selectedPackCodes) as pack}
      <li>{pack.split('-').map(word => word.charAt(0).toUpperCase() + word.slice(1)).join(' ')}: {displayValue(selectedPackCodes[pack])}</li>
    {/each}
  </ul>
</div>