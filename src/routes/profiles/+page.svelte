<script lang="ts">
  import Table from "$lib/Table.svelte";
  import { onMount } from 'svelte';

  /** @type {import('./$types').PageData} */
  export let data;
  console.log("hello", data.names);

  /** @type {import('./$types').ActionData} */
  export let form;

  let name = '';
  let email='';

  // Populate fields with existing data on component mount
  onMount(() => {
    name = data.name;
    email= data.email;
  });
  
  async function handleSubmit(event) {
    event.preventDefault();
  
    try {
      const formData = { name, email };
    

    let endpoint = `your-api-endpoint`;
    let method = 'PUT'; // Since it is an update operation
    
    if (!data.id) {
      throw new Error('No applicant ID found');
    }

    endpoint += `/${data.id}`;

    const response = await fetch(endpoint, {
      method,
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(formData),
    });

    if(!response.ok) {
      throw new Error('Failed to update applicant');
    }

    const responseData = await response.json();
    form.success = true; // Set success if needed
    form.message = 'Updated Applicant!';
  } catch (error){
    console.error ('Error updating applicant:', error); // Handle error if needed
  }
  }
</script>

<Table names={data.names} />

<div
  class="mt-10 pt-10 w-full max-w-xl p-12 mx-auto rounded-lg shadow-xl dark:bg-white/10 bg-white/30 ring-1 ring-gray-900/5 backdrop-blur-lg"
>
  <form method="POST" action="?/create">
  <div class="flex flex-wrap -mx-3 mb-2">
    <div class="flex flex-wrap -mx-3 mb-2">
      <div class="w-full md:w-1/3 px-3 mb-6 md:mb-0">
        <label
          class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
          for="name"
        >
          Name
        </label>
        <input
          class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
          id="name"
          type="text"
          placeholder="Enter name"
          name="name"
        />
      </div>
      <div class="w-full md:w-1/3 px-3 mb-6 md:mb-0">
        <label
          class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
          for="email"
        >
          Email
        </label>
        <input
          class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
          id="email"
          type="text"
          placeholder="Enter email"
          name="email"
        />
      </div>
      <!-- <div class="w-full md:w-1/3 px-3 mb-6 md:mb-0">
            <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-state">
              Email
            </label>
          </div> -->
          <button
        type="submit"
        class="bg-yellow-500 hover:bg-blue-700 text-white font-bold mt-5 py-2 px-2 rounded "
      >
      Create Applicant
      </button>
    <button
        type="submit"
        class="bg-yellow-500 hover:bg-blue-700 text-white font-bold mt-5 ml-2 py-3 px-2 rounded "
      >
      Update applicant
      </button>
  </div>  
</form> 
  {#if form?.success}
    <!-- this message is ephemeral; it exists because the page was rendered in
		response to a form submission. it will vanish if the user reloads -->
    <p class="pt-2">Added New Applicant</p>
  {/if}
</div>
