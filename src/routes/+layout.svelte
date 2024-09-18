<script lang="ts">
    import "../app.css";
</script>

<slot />

<style lang="postcss">
    :global(html) {
        box-sizing: border-box;
    }

    :global(body) {
        font-family: Arial, sans-serif;
    }
</style>
