<script>
    import Toolbar from "./Toolbar.svelte";
</script>

<Toolbar />
<div class="container">
    <slot />
</div>


<style>
    .container {
        padding: 10px 16px;
    }
</style>