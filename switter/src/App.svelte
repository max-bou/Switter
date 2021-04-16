<script>
import { text } from "svelte/internal";
import Message from "./Message.svelte";
export let name;
	let messages=[];
	// "Flag" variable, to hide/show the form fields
	let isVisible = false;

	function addMessage(event){
		console.log(event.detail);
		messages = [event.detail, ...messages];
	}

	const options={
		//CF: mozilla doc @DateTimeFormat
		weekday:"long",
		year:"numeric",
		month:"long",
		day:"numeric",
		hour12:true,
		hour:"numeric",
		minute:"2-digit",
		second:"2-digit"
	};

	const formatter=new Intl.DateTimeFormat("en-US",options)

	function toggle(){
		isVisible = !isVisible;
	}
</script>

<main>
	<h1>{name}</h1>
	<button on:click={toggle}>{isVisible ? 'Hide' : 'Show'}</button>
	{#if isVisible}
	<Message on:message={addMessage}/>
	{/if}
	<div>
		<h2>Messages</h2>
		<!-- svelte-ignore empty-block -->
		{#each messages as message}
		<div class="author">By {message.author} on {formatter.format(message.date)}</div>
		<div>{message.text}</div>
		<hr>
		{/each}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	.author {
		font-weight: bold
	}
</style>