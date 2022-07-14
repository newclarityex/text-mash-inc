<script lang="ts">
    let charactersBuffer = 0;
    let characters = 0;
    let bufferTimeout: number;
    let bufferInterval: number;
    let multiplier = 1;
    $: multiplierProgress = Math.floor(
        (multiplier - Math.floor(multiplier)) * 100
    );
    let maxMultiplier = 3;
    const multiplierModifier = 0.01;
    const multiplierLoss = 0.01;

    setInterval(() => {
        multiplier = Math.max(1, multiplier - multiplierLoss);
    }, 100);

    function updateMultiplier(amt: number) {
        multiplier += amt * multiplierModifier;
        if (multiplier > maxMultiplier) {
            multiplier = maxMultiplier;
        }
    }

    export const increaseBuffer = (amt: number) => {
        updateMultiplier(amt);
        charactersBuffer += amt * multiplier;
        if (bufferTimeout) {
            clearTimeout(bufferTimeout);
        }
        if (bufferInterval) {
            clearInterval(bufferInterval);
        }

        bufferTimeout = window.setTimeout(() => {
            const delay = Math.min((1 / charactersBuffer) * 1000, 50);
            bufferInterval = window.setInterval(() => {
                if (charactersBuffer > 0) {
                    charactersBuffer--;
                    characters++;
                } else {
                    clearInterval(bufferInterval);
                }
            }, delay);
        }, 1000);
    };
</script>

<div class="grow bg-blue-900 flex flex-col items-center">
    <div class="mt-20 mb-8">
        <div class="text-gray-400 text-3xl mb-2">Keys Mashed</div>
        <h1 class="text-white text-center text-5xl font-semibold relative">
            {characters.toFixed(0)}
            {#if charactersBuffer > 0}
                <span class="text-green-300">
                    +{charactersBuffer.toFixed(0)}
                </span>
            {/if}
        </h1>
    </div>
    <h1 class="my-4 text-3xl text-white">Multiplier</h1>
    <div class="w-3/4 bg-white relative">
        <div
            class="absolute h-full top-0 left-0 transition-all duration-200 ease-linear z-0"
            class:bg-green-400={Math.floor(multiplier) === 1}
            class:bg-yellow-400={Math.floor(multiplier) === 2}
            class:bg-orange-400={Math.floor(multiplier) === 3}
            class:bg-red-400={Math.floor(multiplier) === 4}
            class:bg-purple-400={Math.floor(multiplier) === 5}
            class:bg-gray-900={Math.floor(multiplier) === 6}
            style="width: {multiplierProgress}%"
        />
        <div
            class="w-full text-center text-3xl py-2 z-10 relative"
            class:text-black={Math.floor(multiplier) < 6}
            class:text-white={Math.floor(multiplier) >= 6}
        >
            {multiplier.toFixed(1)}x
        </div>
    </div>
</div>
