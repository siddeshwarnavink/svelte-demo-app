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
        <li class="post-item" class:editing={isEditing}>
            <div class="post-item-wrapper">
                <div class="post-detail">
                    <h2>{title}</h2>
                    <p>{content}</p>

                    <Button on:click={toggleEditmodeHandler}>Edit</Button>
                    <Button flat theme="danger" on:click={deletePostHandler}>
                        Delete
                    </Button>
                </div>

                <div class="edit-post">
                    <PostEditor
                        {title}
                        {content}
                        on:submit={saveEditHandler}
                        on:cancel={toggleEditmodeHandler}
                    />
                </div>
            </div>
        </li>
    </Card>
</div>

<style>
    .post-item {
        padding: 10px 14px;
        border-radius: 12px;
        margin-bottom: 10px;
        perspective: 1000px;
        height: 12em;
        transition: height 300ms ease;
    }

    .post-item-wrapper {
        position: relative;
        width: 100%;
        height: 100%;
        transition: transform 0.6s;
        transform-style: preserve-3d;
        width: 50vw;
    }

    .post-item.editing {
        height: 20em;
    }

    .post-item.editing .post-item-wrapper {
        transform: rotateY(180deg);
    }

    .post-item-wrapper .post-detail,
    .post-item-wrapper .edit-post {
        position: absolute;
        width: 100%;
        height: 100%;
        -webkit-backface-visibility: hidden;
        backface-visibility: hidden;
    }

    .edit-post {
        transform: rotateY(180deg);
    }
</style>
