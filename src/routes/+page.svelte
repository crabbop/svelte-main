<h1>CycleRecall</h1>
<p>What is this card?</p>

<script>
  import { onMount } from 'svelte';

  let cardData = {};
  let relatedCards = [];
  let allCards = [];
  let isCorrectGuess = false;
  const packCodes = ['rwr', 'tai', 'su21', 'sg'];

  async function fetchRandomCard() {
    const response = await fetch('https://netrunnerdb.com/api/2.0/public/cards');
    const data = await response.json();
    const filteredCards = data.data.filter(card => packCodes.includes(card.pack_code));
    const randomCard = filteredCards[Math.floor(Math.random() * filteredCards.length)];
    cardData = randomCard;
    isCorrectGuess = false;

    // Fetch related cards
    relatedCards = filteredCards.filter(card => card.type_code === cardData.type_code && card.code !== cardData.code).slice(0, 3);

    // Add the main card to the related cards array
    allCards = [...relatedCards, cardData];

    // Shuffle the array to randomize the positions
    allCards = allCards.sort(() => Math.random() - 0.5);

    // Reset button backgrounds
    document.querySelectorAll('.related-card-button').forEach(button => {
      button.style.backgroundColor = '#f9f9f9';
    });
  }

  onMount(fetchRandomCard);

  function getCardImageUrl(code) {
    return `https://card-images.netrunnerdb.com/v2/large/${code}.jpg`;
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
    background-color: #f9f9f9;
    width: 600px; /* Twice the width of the card image */
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
    height: 12%;
    background-color: rgb(70, 66, 66);
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
    background-color: #f9f9f9;
    margin-left: 20px;
  }
  .refresh-button {
    margin-top: 20px;
  }
  .related-cards {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-top: 20px;
  }
  .related-card {
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 8px;
    background-color: #f9f9f9;
    width: 200px;
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
    background-color: #f9f9f9;
  }
  .related-cards a {
    display: block;
    margin-top: 10px;
    text-decoration: none;
    color: #007acc;
  }
</style>

{#if cardData}
  <div class="card-container">
    <div class="card-image">
      <img src={getCardImageUrl(cardData.code)} alt={cardData.title} />
      {#if !isCorrectGuess}
        <div class="card-image-mask"></div>
      {/if}
    </div>
    <div class="card-details">
      {#if isCorrectGuess}
        <h2>{cardData.title}</h2>
      {/if}
      <p>{@html cardData.text}</p>
      <p>Type: {cardData.type_code}</p>
      <p>Faction: {cardData.faction_code}</p>
    </div>
    <div class="refresh-container">
      <button class="refresh-button" on:click={fetchRandomCard}>Refresh Card</button>
    </div>
  </div>
{/if}

{#if allCards.length > 0}
  <div class="related-cards">
    {#each allCards as card}
      <div class="related-card">
        <button class="related-card-button" on:click={(event) => checkAnswer(event, card.title)}>{card.title}</button>
        <a href={`https://netrunnerdb.com/en/card/${card.code}`} target="_blank">View on NetrunnerDB</a>
      </div>
    {/each}
  </div>
{/if}