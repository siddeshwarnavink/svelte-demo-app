<script>
    import updateArray from "../../util/updateArray";

    export let theme = "default";
    export let flat = false;

    let cssClasses = ["button"];

    if (flat) {
        cssClasses = updateArray(cssClasses, ["flat"]);
    }

    // Button theme
    if ((theme === "default")) {
        cssClasses = updateArray(cssClasses, ["default"]);
    } else if (theme === "danger") {
        cssClasses = updateArray(cssClasses, ["danger"]);
    }

    $: className = cssClasses.join(" ");
</script>

<button class={className}>
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
</style>
