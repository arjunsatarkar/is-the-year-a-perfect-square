<script lang="ts">
    import { DateTime } from "luxon";

    const getCurrentYear = () => DateTime.now().year;
    const getRelative = (year) =>
        DateTime.local(year).toRelative({ locale: "en" });

    let year = $state(getCurrentYear());
    let sqrt = $derived(year ** 0.5);
    let isPerfectSquare = $derived(Number.isInteger(sqrt));
    let lastSqrt = $derived(isPerfectSquare ? sqrt - 1 : Math.floor(sqrt));
    let nextSqrt = $derived(isPerfectSquare ? sqrt + 1 : Math.ceil(sqrt));
    let last = $derived(lastSqrt ** 2);
    let next = $derived(nextSqrt ** 2);
    let nextRelative = $state(getRelative(next));

    setInterval(() => {
        year = getCurrentYear();
        nextRelative = getRelative(next);
    }, 500);
</script>

<div id="container">
    {#if isPerfectSquare}
        <div id="answer">Yes.</div>
        <strong>{sqrt}</strong><sup>2</sup> = {year}.
    {:else}
        <div id="answer">No.</div>
    {/if}
    The last was
    <ruby>{last}<rp> (</rp><rt>{lastSqrt}<sup>2</sup></rt><rp>)</rp></ruby>, and
    the next is
    <ruby>{next}<rp> (</rp><rt>{nextSqrt}<sup>2</sup></rt><rp>)</rp></ruby>, {nextRelative}!
</div>

<style>
    #container {
        text-align: center;
        font-size: 2rem;
    }
    #answer {
        font-size: 5em;
        margin: 0;
        font-weight: bold;
        font-family: sans-serif;
    }
</style>
