<script lang="ts">
    function isFormValid(data: {[fieldName: string]: any}): boolean {
        if(!isRequiredFieldValid(data.email)){
            return false
        }

        if(!isRequiredFieldValid(data.password)) {
            return false
        }
        return true
    }

    function isRequiredFieldValid(value: any){
        console.log(value != null && value !== "", "isRequiredFieldValid")
        return value != null && value !== ""
    }

    // function onSubmit(e: SubmitEvent){
    function onSubmit(e: SubmitEvent){
        const formData = new FormData(e.target as HTMLFormElement);
        const resetForm = e.target as HTMLFormElement;

        console.log(formData)

        const data: Record<string, FormDataEntryValue> = {};
        for (let field of formData) {
            const [key, value] = field;
            data[key] = value;
        }
        if(isFormValid(data)){
            console.log(data)

            // e.target?.reset();
            resetForm.reset()
        } else {
            console.log("Invalid form")
        }
    }


</script>

<main>
    <form on:submit|preventDefault={onSubmit}>
        <div>
            <label for="name">Email</label>
            <input
            type="text"
            id="email"
            name="email"
            value=""
            />
        </div>
        <div>
        <label for="name">Password</label>
        <input
            type="password"
            id="password"
            name="password"
            value=""
        />
    </div>
        <button type="submit">Submit</button>
    </form>
</main>













  <style>
     * {
        box-sizing: border-box;
      }
      form {
        display: flex;
        flex-direction: column;
        width: 300px;
      }

      form > div{
        display: flex;
        justify-content: space-between;
      }

      form > div + * {
        margin-top: 10px;
      }

      :global(html){
        background-color: teal
      }
  </style>