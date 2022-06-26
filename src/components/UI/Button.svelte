<script>
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let theme = "default";
    export let flat = false;
    export let fab = false;
    export let buttonType = 'button';

    function buttonClickedHandler() {
        dispatch("click");
    }
</script>

<button
    class="button"
    class:default={theme === "default"}
    class:danger={theme === "danger"}
    class:flat
    class:fab
    type={buttonType}
    on:click={buttonClickedHandler}
>
    <slot />
</button>

<style>
    .button {
        border: 0;
        padding: 12px 21px;
        cursor: pointer;
        font-size: 16px;
        font-weight: bold;
        border-radius: 12px;
        outline: none;
    }

    .button.default {
        color: #000;
        background-color: rgb(109, 228, 205);
    }

    .button.danger {
        color: #fff;
        background-color: rgb(218, 58, 58);
    }

    .button.flat {
        background-color: transparent !important;
        text-transform: uppercase;
    }

    .button.flat.default {
        color: rgb(109, 228, 205);
    }

    .button.flat.danger {
        color: rgb(218, 58, 58);
    }

    .button.fab {
        border-radius: 100%;
        font-weight: bold;
        /* width: 5px; */
        padding: 12px 21px;
        font-size: 2em;
        position: fixed;
        bottom: 5vh;
        right: 5vw;
    }
</style>
