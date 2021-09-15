<script>
    import { fade, blur, scale, slide, fly } from "svelte/transition";
    import { quintOut } from 'svelte/easing';
    import {getContext} from "svelte";
    export let expense;
    
    // export let removeExpense;
    let displayAmount = true;
    const toggleAmount = () => {
        displayAmount = !displayAmount;
    }

    const removeExpense = getContext("remove")
    const setModifiedExpense = getContext("modify")
</script>
<article class="single-expense">
    <div class="expense-info">
        <h2>
            {expense.name}
            <button class="amount-btn" on:click={toggleAmount}>
                <i class="fas fa-caret-down"/>
            </button>
        </h2>
        {#if displayAmount}
            <!-- <h4 transition:blur> amount : ${expense.amount} </h4> -->
            <!-- <h4 transition:scale> amount : ${expense.amount} </h4> -->
            <h4 in:fly={{x: 100, y: 100, duration: 2000, delay: 500, easing: quintOut}} out:slide> amount : ${expense.amount} </h4>
        {/if}
    </div>
    <div class="expense-buttons">
        <button class="expense-btn" on:click={() => setModifiedExpense(expense.id)} >
            <i class="fas fa-pen"/>
        </button>
        <button class="expense-btn delete-btn" on:click={() => removeExpense(expense.id)}>
            <i class="fas fa-trash"/>
        </button>
    </div>
</article>
