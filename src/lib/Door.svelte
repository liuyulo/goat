<script lang="ts">
    import Card  from '@smui/card';

    export let src: string;
    export let onclick: () => void;
</script>

<Card variant="outlined" padded on:click={onclick} class="cursor-pointer">
    <img {src} alt="door"/>
</Card>
