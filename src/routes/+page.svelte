<script>
	import '@picocss/pico';
	import { onMount } from 'svelte';
	export let data;
    let questions = [];
    
	let index = 0;

	let both = [];
	let diff = [];

	let maxQuestions = 50;

	function diffPressed() {
		let q = questions[index];
		diff.push(q);
		index++;
	}
	function bothPressed() {
		let q = questions[index];
		both.push(q);
		index++;
	}

	$: count = Math.min(questions.length, maxQuestions);

	onMount(() => {
		questions = data.questions
            .sort((a, b) => 0.5 - Math.random())
            .map(x=> x[0].toUpperCase() + x.slice(1));
		console.log('shuffle shuffle');
	});
</script>

<header>
	<h1 class="center">Wedding Game</h1>
</header>
<main class="container">

	{#if index < count}
		<article>
			<h3>Question {index + 1}</h3>
			<p>
				{questions[index]}
			</p>
			<br />
			<div class="grid">
				<button class="outline contrast" on:click={bothPressed}>Both</button>
				<button class="outline secondary" on:click={() => index++}>SKIP</button>
				<button class="outline" on:click={diffPressed}>Different</button>
			</div>
		</article>
	{:else}
		<div class="grid">
			<details open>
				<summary>Both</summary>
                
				{#each both as b}
					<p>{b}</p>
				{/each}
			</details>
			<details open>
				<summary>Different</summary>
				{#each diff as d}
					<p>{d}</p>
				{/each}
			</details>
		</div>
        <button on:click={() => location.reload()}>Reset!</button>
	{/if}

    <div>
        <i>Progress:</i>
        <progress value={index} max={count} />
    </div>
    <details>
        <summary>Settings ⚙️</summary>
        <label for="range">Questions - {count} 
            <input type="range" min="1" max="{questions.length}" value="{count}" id="range" name="range" on:input={(e) => maxQuestions=e.target.value}>
        </label>
    </details>
</main>

<style>
	.center {
		text-align: center;
	}
</style>
