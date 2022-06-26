<script>
    import { fade, fly } from "svelte/transition";
    import { createEventDispatcher } from "svelte";

    import Button from "../UI/Button.svelte";
    import Card from "../UI/Card.svelte";
    import PostEditor from "./PostEditor.svelte";

    const dispatch = createEventDispatcher();

    export let id;
    export let title;
    export let content;

    let isEditing = false;

    function toggleEditmodeHandler() {
        isEditing = !isEditing;
    }

    function saveEditHandler(event) {
        toggleEditmodeHandler();

        dispatch("editPost", {
            id,
            title: event.detail.title,
            content: event.detail.content,
        });
    }

    function deletePostHandler() {
        dispatch("deletePost", id);
    }
</script>

<div in:fly={{ y: 200, duration: 300 }} out:fade>
    <Card>
        <li class="post-item">
            {#if !isEditing}
                <h2>{title}</h2>
                <p>{content}</p>
            {:else}
                <PostEditor
                    {title}
                    {content}
                    on:submit={saveEditHandler}
                    on:cancel={toggleEditmodeHandler}
                />
            {/if}

            {#if !isEditing}
                <Button on:click={toggleEditmodeHandler}>Edit</Button>
                <Button flat theme="danger" on:click={deletePostHandler}>
                    Delete
                </Button>
            {/if}
        </li>
    </Card>
</div>

<style>
    .post-item {
        padding: 10px 14px;
        border-radius: 12px;
        margin-bottom: 10px;
    }
</style>
