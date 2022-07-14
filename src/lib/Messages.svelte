<script lang="ts">
    import { onMount } from "svelte";
    export let increaseBuffer: (amt: number) => void;
    let currentCharacters = 0;

    let messages: {
        text: string;
        incoming: boolean;
    }[] = [];

    onMount(() => {
        const messageInput = document.getElementById(
            "message-input"
        ) as HTMLInputElement;
        messageInput.onpaste = (e) => {
            e.preventDefault();
            return false;
        };
    });

    function handleMessageSubmit(event: KeyboardEvent) {
        if (event.key !== "Enter") {
            return;
        }

        event.preventDefault();
        const element = event.target as HTMLInputElement;
        const text = element.value;
        if (text.length === 0) {
            return;
        }
        increaseBuffer(text.length);
        currentCharacters = 0;
        messages = [
            ...messages,
            {
                text,
                incoming: false,
            },
        ];
        if (messages.length > 50) {
            messages.shift();
        }
        element.value = "";
    }

    function handleInputUpdate(event: Event) {
        const element = event.target as HTMLInputElement;
        currentCharacters = element.value.length;
    }
</script>

<div
    class="h-full w-1/3 bg-gray-800 flex items-center justify-center shadow-2xl"
>
    <div
        class="h-4/5 w-2/3 flex flex-col items-center bg-stone-300 rounded-3xl px-4 relative overflow-hidden"
    >
        <div
            class="absolute top-0 left-0 w-full bg-blue-900 shadow-2xl py-6 text-center text-3xl text-white z-10 font-semibold"
        >
            UwU
        </div>
        <div class="grow relative w-full overflow-hidden">
            <ul
                class="absolute bottom-0 w-full flex flex-col justify-end items-end"
            >
                {#each messages as message}
                    <li
                        class="px-2 py-1.5 text-white bg-blue-600 my-2 mr-4 ml-20 rounded-2xl max-w-[80%] wrap"
                    >
                        {message.text}
                    </li>
                {/each}
            </ul>
        </div>
        <div class="w-full h-24 flex justify-center items-center relative mt-2">
            <div class="absolute -top-2 w-full left-0 text-center text-2xl">
                {currentCharacters}
            </div>
            <input
                type="text"
                class="w-4/5 rounded-2xl px-4 py-2 text-xl"
                placeholder="Type a message..."
                on:keypress={handleMessageSubmit}
                on:input={handleInputUpdate}
                id="message-input"
            />
        </div>
    </div>
</div>

<style>
    .wrap {
        word-wrap: break-word;
    }
</style>
