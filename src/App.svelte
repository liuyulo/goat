<script lang="ts">
    import Textfield from '@smui/textfield';
    import Card from '@smui/card';

    import { fade } from 'svelte/transition';

    import car from './assets/car.png';
    import goat from './assets/goat.png';
    import door from './assets/door.png';

    // update localStorage
    const key = 'goat';
    let value = JSON.parse(localStorage.getItem(key) || '10');
    $: localStorage.setItem(key, JSON.stringify(value));

    let select: number, opened: boolean;
    // reset select and opened if value changes
    $: if (value) {
        select = undefined;
        opened = false;
    }
    $: doors = Array.from(Array(value), (_) => false);
    $: answer = (Math.random() * value) | 0;

    function choose(n: number) {
        if (opened) return;
        select = n;
        opened = true;
        // if selected is correct, choose another to keep closed
        while (n == answer) {
            n = (Math.random() * value) | 0;
        }
        doors = doors.map((_, i) => i != answer && i != n);
    }

    function reveal(_: number) {
        doors = Array.from(doors, (_) => true);
    }

    const onclick = (i: number) => () => (opened ? reveal : choose)(i);
</script>

<main class="p-4">
    <Textfield bind:value type="number" />
    <div
        class="mt-4 grid grid-cols-4 md:grid-cols-[repeat(auto-fill,200px)] gap-4"
    >
        {#each doors as d, i}
            {@const hit = i == answer}
            {@const highlight = i == select}
            {@const src = d ? (hit ? car : goat) : door}
            <Card
                variant="outlined"
                padded
                on:click={onclick(i)}
                class={`cursor-pointer ${highlight ? 'highlight' : ''}`}
            >
                {#key src}
                    <img in:fade {src} alt="door" />
                {/key}
            </Card>
        {/each}
    </div>
</main>

<style lang="sass">

</style>
