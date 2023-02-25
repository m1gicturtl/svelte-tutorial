<script>
    import {v4 as uuidv4} from 'uuid';
    // import { createEventDispatcher } from 'svelte';
    import { FeedbackStore } from '../stores';
    import Card from "./Card.svelte";
    import Button from "./Button.svelte";
    import RatingSelect from "./RatingSelect.svelte";

    // const dispatch = createEventDispatcher();

    let text = "";
    let rating = 10;
    let btnDisabled = true;
    let min = 10;
    let msg; 

    const handleSelect = e => rating = e.detail;

    const handleInput = () => {
        if (text.length <= min) {
            msg = `You need ${min - text.length} more characters`;
            btnDisabled = true;
        } else {
            msg = null;
            btnDisabled = false;
        }
    }

    const handleSubmit = () => {
        if(text.trim().length >= min) {
            const newFeedback = {
                id: uuidv4(),
                text,
                rating: +rating
            }

            // dispatch('add-feedback', newFeedback);
            FeedbackStore.update((currentFeedback) => {
                return [newFeedback, ...currentFeedback];
            });

            text='';
        }
    }
</script>

<Card>
    <header>
        <h2>Feedback</h2>
    </header>
    <form on:submit|preventDefault={handleSubmit}>
        <RatingSelect on:rating-select={handleSelect}/>
        <div class="input-group">
            <input type="text" on:input={handleInput} bind:value = {text} placeholder="Gib review" />
            <Button disabled={btnDisabled} type="submit">Sned</Button>
        </div>
        {#if msg}
            <div class="msg">
                {msg}
            </div>
        {/if}
    </form>
</Card>

<style>
    header {
        max-width: 400px;
        margin: auto;
    }
    header h2 {
        font-size: 22px;
        font-weight: 600;
        text-align: center;
    }
    .input-group {
        display: flex;
        flex-direction: row;
        border: 1px solid #ccc;
        padding: 8px 10px;
        border-radius: 8px;
        margin-top: 15px;
    }
    input {
        flex-grow: 2;
        border: none;
        font-size: 16px;
    }
    input:focus {
        outline: none;
    }
    .msg {
        padding-top: 10px;
        text-align: left;
        color: rebeccapurple;
    }
</style>
