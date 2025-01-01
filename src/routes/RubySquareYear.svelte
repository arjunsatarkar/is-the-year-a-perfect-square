<script lang="ts">
    import Square from "./Square.svelte";

    let { sqrt }: { sqrt: number } = $props();
</script>

<ruby>{sqrt ** 2}<rp> (</rp><rt>{sqrt}<Square /></rt><rp>)</rp></ruby>
