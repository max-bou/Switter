<script>
    import {createEventDispatcher} from 'svelte';
    const dispatch = createEventDispatcher();

    	let message = "";
        let author = "";
        let maxLength = 140;

        // $: is for a listened variable, it will be update with every modification
        $: nbChar = message.length;
        // This listened variable is for disable the "send" button when the message is not valid
        $: disabled = message.length > maxLength || message.length == 0 ? true : false;


        function saveMessage(){
		// Add every new message in the messages aray
		const newMessage={
			id:Date.now(),
			text:message,
            // Here we add the author from the parent comp, else set "anonymous" as default
			author: author || "anonymous",
            date: new Date()
		};

        dispatch('message', newMessage);
		// Reset variables to clean the text fields
		message="";
        author="";
	}
</script>

<style>
.text{
    width: 382px;
}

.alert{
    color: orangered;
}
</style>

<main>
    <input class="text" type="text" bind:value={author}/>
    <br>
    <textarea cols="50" rows="5" bind:value={message}/>
	<br>
    <!-- svelte-ignore missing-declaration -->
	<button on:click={saveMessage} disabled={disabled}>Send</button>
    <!-- Conditional applying of the ".alert" class to warn the user about his message validity -->
    <span class:alert={nbChar>maxLength}>{nbChar}</span>
    {#if disabled}
        <span class="alert">Invalid message !</span>
    {/if}
</main>