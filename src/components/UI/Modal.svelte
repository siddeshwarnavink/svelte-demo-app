<script>
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
{/if}

<div class="modal" class:open={isOpen}>
    <div class="modal-container">
        <div class="modal-close">
            <Button on:click={onCloseHandler}>X</Button>
        </div>
        <slot />
    </div>
</div>

<style>
    .modal {
        position: fixed;
        background-color: #fff;
        z-index: 200;
        left: 35vw;
        width: 30vw;
        border-radius: 12px;
        display: none;
    }

    .modal.open {
        display: block;
    }

    .modal-container {
        padding: 10px;
    }

    .modal-close {
        float: right;
    }
</style>
