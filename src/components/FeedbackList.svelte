<script>
    // import {onMount, onDestroy } from 'svelte';
    import { FeedbackStore } from '../stores';
    import { fade, scale } from "svelte/transition";
    import FeedbackItem from "./FeedbackItem.svelte";
    
    // let feedback = [];

    // // when subscribing to a store, it will return a function that we can call to unsubscribe
    // const unsubscribe = FeedbackStore.subscribe((data) => feedback = data);

    // onMount(() => {
    //     console.log('FeedbackList mounted');
    // });

    // onDestroy(() => {
    //     console.log('FeedbackList destroyed');
    //     unsubscribe();
    // });

</script>

<!-- $FeedbackStore does the same as the commented code above does but in a short(er) way -->
{#each $FeedbackStore as fb (fb.id)}
    <div in:scale="{{ duration: 500 }}" out:fade="{{ duration: 500 }}">
        <FeedbackItem item={fb} />
    </div>
{/each}
