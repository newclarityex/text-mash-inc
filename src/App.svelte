<script lang="ts">
    import Messages from "./lib/Messages.svelte";
    import Game from "./lib/Game.svelte";

    let charactersBuffer = 0;
    let increaseBuffer: (amt: number) => void;

    let increaseBufferWrapper = (amt: number) => {
        increaseBuffer(amt);
    };
</script>

<main class="flex flex-row">
    <Messages bind:increaseBuffer={increaseBufferWrapper} />
    <Game bind:increaseBuffer />
</main>

<style>
    main {
        width: 100%;
        height: 100%;
    }
</style>
