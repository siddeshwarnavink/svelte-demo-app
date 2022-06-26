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

    <Button buttonType="submit">Save</Button>
    <Button theme="danger" on:click={cancelEditHandler}>Cancel</Button>
</form>

<style>
    .form-field {
        display: flex;
        flex-direction: column;
        width: 28vw;
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
        font-family: 'Nunito', sans-serif;
    }

    .form-field textarea {
        resize: none;
        height: 125px;
    }
</style>
