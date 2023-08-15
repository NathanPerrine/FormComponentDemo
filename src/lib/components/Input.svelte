<script lang="ts">
    import { getContext } from 'svelte';
    import type { FocusEventHandler } from 'svelte/elements';
    export let type = 'text';
    export let label: string;
    export let name: string;
    const onBlur: FocusEventHandler<HTMLInputElement> = getContext('form');
</script>
    <label for={name}>{label}</label>
    <input {name} {type} on:blur={onBlur} />