<script>
    import Button from "../UI/Button.svelte";
    import Card from "../UI/Card.svelte";

    export let title;
    export let content;
</script>

<Card>
    <li class="post-item">
        <h1>{title}</h1>
        <p>{content}</p>

        <Button>Edit</Button>
        <Button flat theme="danger">Delete</Button>
    </li>
</Card>

<style>
    .post-item {
        padding: 10px 14px;
        border-radius: 12px;
        margin-bottom: 10px;
    }

    .post-item h1 {
        margin: 0;
    }
</style>
