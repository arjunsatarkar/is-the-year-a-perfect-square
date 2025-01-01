<script lang="ts">
    import { DateTime } from "luxon";
    import RubySquareYear from "./RubySquareYear.svelte";
    import Square from "./Square.svelte";

    const getCurrentYear = () => DateTime.now().year;
    const getRelative = (year: number) =>
        DateTime.local(year).toRelative({ locale: "en" })!;

    let year = $state(getCurrentYear());
    let sqrt = $derived(year ** 0.5);
    let isPerfectSquare = $derived(Number.isInteger(sqrt));
    let lastSqrt = $derived(isPerfectSquare ? sqrt - 1 : Math.floor(sqrt));
    let nextSqrt = $derived(isPerfectSquare ? sqrt + 1 : Math.ceil(sqrt));
    let next = $derived(nextSqrt ** 2);
    let nextRelative = $state("");

    const update = () => {
        year = getCurrentYear();
        nextRelative = getRelative(next);
    };
    update();
    setInterval(update, 500);
</script>

<div id="mainDisplay">
    <h1>Is the year a perfect square?</h1>
    {#if isPerfectSquare}
        <div id="answer">Yes.</div>
        <strong>{sqrt}</strong><Square /> = {year}.
    {:else}
        <div id="answer">No.</div>
    {/if}
    The last was
    <RubySquareYear sqrt={lastSqrt} /> and the next is
    <RubySquareYear sqrt={nextSqrt} />, {nextRelative}!
</div>

<div id="sourceLinkContainer">
    <a href="https://github.com/arjunsatarkar/is-the-year-a-perfect-square">source code</a>
</div>

<style>
    #mainDisplay {
        text-align: center;
        font-size: 2rem;
    }
    h1 {
        font-size: inherit;
        margin: 0;
        font-weight: normal;
    }
    #answer {
        font-size: 5em;
        margin: 0;
        font-weight: bold;
        font-family: sans-serif;
    }
    #sourceLinkContainer {
        text-align: right;
    }
</style>
