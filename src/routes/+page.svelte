<script lang="ts">
    import { Validators } from '../lib/components/Validators';
    import Form from '../lib/components/Form.svelte'
    import Input from '../lib/components/Input.svelte'
    import Error from '../lib/components/Error.svelte'

    let formEl: HTMLFormElement;
    let form = {
        email: {
            validators: [Validators.required],
        },
        password: {
            validators: [Validators.required, Validators.minLength(6)],
        },
    };

    function onSubmit(e: any) {
        if (e?.detail?.valid) {
            console.log(e.detail.data);
            setTimeout(() => formEl.reset(), 1000)
        } else {
            console.log('Invalid Form');
        }
    }


</script>

<main>
    <form on:submit|preventDefault={onSubmit} bind:this={formEl}>
        <div>
            <Input label="Email" name="email" />
            <Error
                fieldName="email"
                errorKey="required"
                message="Email is required"
            />
        </div>
        <div>
            <Input label="Password" name="password" />
            <Error
                fieldName="password"
                errorKey="required"
                message="Password is required"
            />
            <Error fieldName="password" errorKey="minLength" />
        </div>
        <button type="submit">Submit</button>
    </form>
</main>

<style>
    * {
        box-sizing: border-box;
    }

    :global(html){
        background-color: teal
    }
</style>