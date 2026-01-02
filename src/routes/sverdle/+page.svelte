<svelte:head>
  <title>Game Memory Card</title>
</svelte:head>
<script lang="ts">
  const emojis = ['🍎','🍌','🍇','🍉','🍓','🍒','🥝','🍍','🥑','🍋','🍑','🍈','🍐','🥭','🍊','🍎','🍌','🍇'];

  let level: number = 1;
  let size: number = 2;
  let cards: string[] = [];
  let opened: number[] = [];
  let matched: boolean[] = [];
  let score: number = 0;

  function setupLevel(): void {
    size = level === 1 ? 2 : level === 2 ? 4 : 6;
    const total = size * size;
    cards = emojis.slice(0, total / 2);
    cards = [...cards, ...cards].sort(() => Math.random() - 0.5);
    matched = Array(cards.length).fill(false);
    opened = [];
  }

  setupLevel();

  function openCard(index: number): void {
    if (opened.length === 2 || matched[index] || opened.includes(index)) return;

    opened = [...opened, index];

    if (opened.length === 2) {
      const [a, b] = opened;
      if (cards[a] === cards[b]) {
        matched[a] = matched[b] = true;
        score += 10;
        opened = [];

        if (matched.every(m => m)) {
          level++;
          if (level <= 3) setupLevel();
          else alert("🎉 Semua level selesai!");
        }
      } else {
        setTimeout(() => opened = [], 800);
      }
    }
  }
</script>

<div class="game">
  <h1>🧠 Memory Card</h1>
  <p>Level: {level}</p>
  <p>Skor: {score}</p>

  <div class="grid" style="grid-template-columns: repeat({size}, 1fr)">
    {#each cards as card, i}
      <button class="card" on:click={() => openCard(i)}>
        {opened.includes(i) || matched[i] ? card : '❓'}
      </button>
    {/each}
  </div>
</div>

<style>
  .game {
    max-width: 360px;
    margin: 50px auto;
    text-align: center;
  }

  .grid {
    gap: 10px;
    display: grid;
  }

  .card {
    padding: 15px;
    font-size: 22px;
    border-radius: 8px;
    cursor: pointer;
    background: #f0f0f0;
    border: 1px solid #333;
  }
</style>
