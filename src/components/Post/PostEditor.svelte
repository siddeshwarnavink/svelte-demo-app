<script>
    import { createEventDispatcher } from "svelte";

    import Button from "../UI/Button.svelte";

    const dispatch = createEventDispatcher();

    export let title = "";
    export let content = "";

    function onSubmitHandler() {
        dispatch("submit", { title, content });
    }

    function cancelEditHandler() {
        dispatch("cancel", { title, content });
    }
</script>

<form on:submit|preventDefault={onSubmitHandler}>
    <div class="form-field">
        <label for="titleInput">Title</label>
        <input name="titleInput" bind:value={title} />
    </div>

    <div class="form-field">
        <label for="contentInput">Content</label>
        <textarea name="contentInput" bind:value={content} />
    </div>

    <Button>Save</Button>
    <Button theme="danger" on:click={cancelEditHandler}>Cancel</Button>
</form>

<style>
    .form-field {
        display: flex;
        flex-direction: column;
        width: 30vw;
        margin-bottom: 12px;
    }

    .form-field label {
        font-weight: bold;
    }

    .form-field input,
    .form-field textarea {
        border: 1px solid #ccc;
        padding: 10px;
        outline: none;
        font-size: 16px;
    }

    .form-field textarea {
        resize: none;
        height: 125px;
    }
</style>
