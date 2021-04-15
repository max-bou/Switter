<script>
    import {createEventDispatcher} from 'svelte';
    const dispatch = createEventDispatcher();

    	let message = "";
        let author = "";
        let maxLength = 140;

        // Utiliser $: pour surveiller cette variable, à chaque changement elle sera modifiée
        $: nbChar = message.length;
        // On va désactiver le bouton send quand le message est trop long
        $: disabled = message.length > maxLength || message.length == 0 ? true : false;


        function saveMessage(){
		// Chaque nouveau message sera stocké dans le tableau messages 
		const newMessage={
			id:Date.now(),
			text:message,
            // Ici on récupère l'author définit dans le comp parent
			author: author || "anonymous",
            date: new Date()
		};

        dispatch('message', newMessage);
		// Reseter les variables pour vider les champs de saisie
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
    <!--Application de la class alert de manière conditionelle pour signaler qu'on a dépassé la limite de charactère-->
    <span class:alert={nbChar>maxLength}>{nbChar}</span>
    {#if disabled}
        <span class="alert">Invalid message !</span>
    {/if}
</main>