<h1>CycleRecall</h1>
<p>What is this card?</p>

<script>
  import { onMount } from 'svelte';

  let cardData = {};
  let relatedCards = [];
  let allCards = [];
  let isCorrectGuess = false;
  const packCodes = ['rwr', 'tai', 'su21', 'sg'];
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
  let isValidSelection = true;

  async function fetchRandomCard() {
    const response = await fetch('https://netrunnerdb.com/api/2.0/public/cards');
    const data = await response.json();
    let filteredCards = data.data.filter(card => packCodes.includes(card.pack_code));

    const selectedSideCodeKeys = Object.keys(selectedSideCodes).filter(key => selectedSideCodes[key]);
    if (selectedSideCodeKeys.length > 0) {
      filteredCards = filteredCards.filter(card => selectedSideCodeKeys.includes(card.side_code));
    }

    const selectedTypeCodeKeys = Object.keys(selectedTypeCodes).filter(key => selectedTypeCodes[key]);
    if (selectedTypeCodeKeys.length > 0) {
      filteredCards = filteredCards.filter(card => selectedTypeCodeKeys.includes(card.type_code));
    }

    const selectedFactionCodeKeys = Object.keys(selectedFactionCodes).filter(key => selectedFactionCodes[key]);
    if (selectedFactionCodeKeys.length > 0) {
      filteredCards = filteredCards.filter(card => selectedFactionCodeKeys.includes(card.faction_code));
    }

    if (filteredCards.length === 0) {
      isValidSelection = false;
      cardData = {};
      allCards = [];
    } else {
      isValidSelection = true;
      const randomCard = filteredCards[Math.floor(Math.random() * filteredCards.length)];
      cardData = randomCard;
      isCorrectGuess = false;

      // Fetch related cards
      relatedCards = filteredCards.filter(card => card.type_code === cardData.type_code && card.code !== cardData.code).slice(0, numberOfButtons - 1);

      // Add the main card to the related cards array
      allCards = [...relatedCards, cardData];

      // Shuffle the array to randomize the positions
      allCards = allCards.sort(() => Math.random() - 0.5);

      // Reset button backgrounds and text color
      document.querySelectorAll('.related-card-button').forEach(button => {
        button.style.backgroundColor = '#808080'; // Darker grey background color
        button.style.color = '#fff'; // White text color
      });
    }
  }

  onMount(fetchRandomCard);

  function getCardImageUrl(code) {
    return `https://card-images.netrunnerdb.com/v2/large/${code}.jpg`;
  }

  function handleImageError(event) {
    event.target.src = 'default-image.png'; // Updated path to your fallback image
  }

  function checkAnswer(event, title) {
    if (title === cardData.title) {
      event.target.style.backgroundColor = 'green';
      isCorrectGuess = true;
    } else {
      event.target.style.backgroundColor = 'red';
    }
  }
</script>

<style>
  :global(body) {
    background-color: #727272; /* Light grey background color */
    color: #000; /* Default text color */
    margin: 0;
    font-family: Arial, sans-serif;
  }

  .card-container {
    display: flex;
    align-items: flex-start;
    gap: 20px;
    justify-content: center;
    margin-top: 50px;
  }
  .card-details {
    border: 1px solid #ccc;
    padding: 20px;
    border-radius: 8px;
    background-color: #a9a9a9; /* Darker grey background color */
    width: 600px; /* Twice the width of the card image */
  }
  .card-title {
    font-size: 1.5em;
    font-weight: bold;
    margin-bottom: 10px;
  }
  .card-title-blurred {
    filter: blur(10px); /* Apply blur effect */
  }
  .card-image {
    max-width: 300px;
    border-radius: 8px;
    position: relative;
  }
  .card-image-mask {
    position: absolute;
    top: 0;
    right: 0;
    width: 100%;
    height: 12%; /* Cover the entire image */
    backdrop-filter: blur(6px); /* Apply blur effect */
    z-index: 1;
  }
  .refresh-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border: 1px solid #ccc;
    padding: 20px;
    border-radius: 8px;
    background-color: #a9a9a9; /* Darker grey background color */
    margin-bottom: 20px; /* Add margin to separate from options */
    width: 250px; /* Same width as options panel */
  }
  .refresh-button {
    width: 100%; /* Full width */
    margin-top: 20px;
    background-color: #808080; /* Darker grey background color */
    color: #fff; /* White text color */
    border: none;
    padding: 10px;
    border-radius: 8px;
    cursor: pointer;
  }
  .related-cards {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    margin-top: 20px;
  }
  .related-cards-title {
    font-size: 1.2em;
    font-weight: bold;
  }
  .related-card-buttons {
    display: flex;
    justify-content: center;
    gap: 20px;
  }
  .related-card {
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 8px;
    background-color: #a9a9a9; /* Darker grey background color */
    width: 150px; /* 25% of card-details width */
    text-align: center;
  }
  .related-card button {
    width: 100%;
    height: 100%;
    background: none;
    border: none;
    cursor: pointer;
    padding: 10px;
    border-radius: 8px;
    background-color: #808080; /* Darker grey background color */
    color: #fff; /* White text color */
  }
  .related-cards a {
    display: block;
    margin-top: 10px;
    text-decoration: none;
    color: #007acc;
  }
  .dropdown-container {
    display: flex;
    align-items: center;
    justify-content: left;
    margin-top: 20px;
  }
  .dropdown-container label {
    margin-right: 10px;
  }
  .filter-container {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-top: 20px;
  }
  .filter-container div {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .filter-container label {
    margin-bottom: 5px;
  }
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
</style>

{#if !isValidSelection}
  <p style="color: red; text-align: center; font-size: medium;">Invalid Guess Options - Change Selections</p>
{/if}

<div class="card-container">
  <div>
    <div class="refresh-container">
      <button class="refresh-button" on:click={fetchRandomCard}>Refresh Card</button>
    </div>
    <div class="options-container">
      <h3>Options</h3>
      <div class="option-group">
        <label>Guess' displayed:</label>
        <label><input type="radio" bind:group={numberOfButtons} value="2" on:change={fetchRandomCard}> 2</label>
        <label><input type="radio" bind:group={numberOfButtons} value="3" on:change={fetchRandomCard}> 3</label>
        <label><input type="radio" bind:group={numberOfButtons} value="4" on:change={fetchRandomCard}> 4</label>
      </div>
      <div class="option-group">
        <label>Side:</label>
        <label><input type="checkbox" bind:checked={selectedSideCodes.runner} on:change={fetchRandomCard}> Runner</label>
        <label><input type="checkbox" bind:checked={selectedSideCodes.corp} on:change={fetchRandomCard}> Corp</label>
      </div>
      <div class="option-group">
        <label>Faction:</label>
        {#each Object.keys(selectedFactionCodes) as faction}
          <label><input type="checkbox" bind:checked={selectedFactionCodes[faction]} on:change={fetchRandomCard}> 
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
          <label><input type="checkbox" bind:checked={selectedTypeCodes[type]} on:change={fetchRandomCard}> {type.charAt(0).toUpperCase() + type.slice(1)}</label>
        {/each}
      </div>
    </div>
  </div>
  <div class="card-image">
    <img src={getCardImageUrl(cardData.code)} alt={cardData.title} on:error={handleImageError} />
    {#if !isCorrectGuess}
      <div class="card-image-mask"></div>
    {/if}
  </div>
  <div>
    <div class="card-details">
      <h2 class="card-title {isCorrectGuess ? '' : 'card-title-blurred'}">{isCorrectGuess ? cardData.title : 'XXXX - XXXX'}</h2>
      <p>{@html cardData.text}</p>
      <p>Type: {cardData.type_code}</p>
      <p>Faction: {cardData.faction_code}</p>
    </div>
    {#if allCards.length > 0}
      <div class="related-cards">
        <div class="related-cards-title">Guess the title of the displayed card</div>
        <div class="related-card-buttons">
          {#each allCards as card}
            <div class="related-card">
              <button class="related-card-button" on:click={(event) => checkAnswer(event, card.title)}>{card.title}</button>
              <a href={`https://netrunnerdb.com/en/card/${card.code}`} target="_blank">View on NetrunnerDB</a>
            </div>
          {/each}
        </div>
      </div>
    {/if}
  </div>
</div>