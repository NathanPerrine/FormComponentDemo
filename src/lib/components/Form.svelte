<script lang="ts">
    import { setContext } from 'svelte';
    import { Validators } from './Validators';
    import type { ValidatorFn, ValidatorResult } from './Validators';
    import { createEventDispatcher } from 'svelte';
    import { writable } from 'svelte/store';


    let formEl: HTMLFormElement;
    export let form: {
        [inputName: string]: {
            validators: ValidatorFn[];
        };
    } = {};

    const dispatch = createEventDispatcher();

    interface validationError {
        [key: string]: any;
    }
    let errors: validationError = writable({});



    function isFormValid(): boolean {
        return !Object.values(errors).some((field) =>
            Object.values(field).some((errorObject: any) => errorObject.error),
        );
    }

    function validateForm(data: { [inputName: string]: any }): void {
        Object.keys(data).forEach((field) => validateField(field, data[field]));
    }

    function validateField(field: string, value: any) {
        form[field]?.validators &&
            form[field].validators.forEach((fn) => {
                const error = fn(value);
                errors.update((e: any) => {
                    e[field] = { ...e[field], ...error };
                    return e;
                })
            });
    }



    function onSubmit(e: SubmitEvent){
        const formData = new FormData(e.target as HTMLFormElement);
        const resetForm = e.target as HTMLFormElement;

        // console.log(formData)

        const data: Record<string, FormDataEntryValue> = {};
        for (let field of formData) {
            const [key, value] = field;
            data[key] = value;
        }

        validateForm(data);

        return dispatch('submit', { valid: isFormValid(), data });

        // if(isFormValid()){
        //     console.log(data)
        //     resetForm.reset()
        // } else {
        //     console.log("Invalid form")
        // }
    }

    export function reset() {
        formEl.reset()
    }

    setContext('form', { errors, onBlur })

    function onBlur(e: Event){
        validateField((e.target as HTMLInputElement).name, (e.target as HTMLInputElement).value)
    }
</script>

<form on:submit|preventDefault={onSubmit} bind:this={formEl}>
    <slot />
</form>

<style>
    form {
        display: flex;
        flex-direction: column;
        width: 300px;
    }

    :global(form > div){
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
    }

    :global(form > div + *) {
        margin-top: 10px;
    }
</style>