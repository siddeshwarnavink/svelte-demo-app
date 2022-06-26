<script>
    import { fly } from "svelte/transition";
    import { createEventDispatcher } from "svelte";

    import Backdrop from "./Backdrop.svelte";
    import Button from "./Button.svelte";

    export let isOpen;

    const dispatch = createEventDispatcher();

    function onCloseHandler() {
        dispatch("close");
    }
</script>

{#if isOpen}
    <Backdrop />

    <div class="modal" transition:fly={{ y: -200, duration: 500 }}>
        <div class="modal-container">
            <div class="modal-close">
                <Button on:click={onCloseHandler}>X</Button>
            </div>
            <slot />
        </div>
    </div>
{/if}

<style>
    .modal {
        position: fixed;
        background-color: #fff;
        z-index: 200;
        left: 35vw;
        width: 30vw;
        border-radius: 12px;
    }

    .modal-container {
        padding: 10px;
    }

    .modal-close {
        float: right;
    }
</style>
