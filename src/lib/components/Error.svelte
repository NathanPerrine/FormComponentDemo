<script lang="ts">
    import { getContext } from 'svelte';
    const  errors  = getContext<HTMLFormElement>('form');
    export let message: string | null = null;
    export let fieldName: string;
    export let errorKey: any;
</script>

{#if $errors?.[fieldName]?.[errorKey]?.error}
    <p class="error-message">{message || $errors[fieldName][errorKey].message}</p>
{/if}

<style>
    .error-message {
        color: tomato;
        flex: 0 0 100%;
        margin: 0 2px;
        font-size: 0.8rem;
    }
</style>