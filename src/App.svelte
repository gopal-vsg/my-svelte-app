<script>
  import { createClient } from '@supabase/supabase-js'
  import { onMount } from 'svelte'

  const supaurl = 'https://aigrlgsdtaxhicbspfqs.supabase.co';
  const supaapi = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImFpZ3JsZ3NkdGF4aGljYnNwZnFzIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTcyMTc4NjksImV4cCI6MjAzMjc5Mzg2OX0.9JfpozFSgXpu5_uMN06oK5-Oq6uD2NYclcluDtJFvL8';

  const supabase = createClient(supaurl, supaapi)
  
  let userid = ''
  let password = ''
  let message = ''
  let queryid = ''
  let userdetails = null

  async function add() {
    const { data, error } = await supabase.from('users').insert([{ userid: userid, password: password }])
    if (error) {
      message = `Error : ${error.message}`
    } else {
      console.log("data entered successfully")
      message = 'Data inserted successfully'
      userid = ''
      password = ''
    }
  }

  async function fetch() {
    const { data, error } = await supabase.from('users').select('*').eq('userid', queryid).single()
    if (error) {
      message = 'Hey something is wrong'
    } else {
      console.log("done")
      userdetails = data
      document.getElementById('queryid').innerText=''
    }
  }

  let currentid = ''
  let newid = ''
  let newpass = ''

  async function edit(){
    const {data,error} = await supabase.from('users').update({userid : newid, password : newpass}).eq('userid',currentid)
    if(error){
      console.log("error : ",error)
      message = `Error : ${error.message}`
    }
    else{
      message = 'Data updated successfully'
      currentid = ''
      newid = ''
      newpass = ''
    }
  }

  async function del(){
    const {data,error} = await supabase.from('users').delete().eq('userid',currentid)
    if(error){
      message = 'Something is messed up'
    }
    else{
      message = 'Data deleted successfully'
      currentid = ''
    }
  }
</script>

<main class="container">
  <h1 class="title">the CRED Operations</h1>
  
  <section class="section">
    <h2 class="subtitle">Create</h2>
    <form on:submit|preventDefault={add} class="form">
      <label class="label" for="userid">Your User ID:</label>
      <input type="text" id="userid" class="input" bind:value={userid}>

      <label class="label" for="password">Your Password:</label>
      <input type="password" id="password" class="input" bind:value={password}>

      <button type="submit" class="button">Add User</button>
    </form>

    {#if message}
      <p class="message">{message}</p>
    {/if}
  </section>

  <section class="section">
    <h2 class="subtitle">Read</h2>
    <label class="label" for="queryid">Enter User ID to Fetch:</label>
    <input type="text" id="queryid" class="input" bind:value={queryid}>

    <button on:click={fetch} class="button">Fetch User Details</button>

    {#if userdetails}
      <div class="details">
        <h3>User Details</h3>
        <p>User ID: {userdetails.userid}</p>
        <p>User Password: {userdetails.password}</p>
      </div>
    {:else if message}
      <p class="message">{message}</p>
    {/if}
  </section>

  <section class="section">
    <h2 class="subtitle">Edit</h2>
    <form on:submit|preventDefault={edit} class="form">
      <label class="label" for="currentid">Current User ID:</label>
      <input type="text" id="currentid" class="input" bind:value={currentid}>

      <label class="label" for="newUserId">New User ID:</label>
      <input type="text" id="newUserId" class="input" bind:value={newid}>

      <label class="label" for="newpass">New Password:</label>
      <input type="password" id="newpass" class="input" bind:value={newpass}>

      <button type="submit" class="button">Edit User</button>
    </form>
  </section>

  <section class="section">
    <h2 class="subtitle">Delete</h2>
    <label class="label" for="deleteid">User ID to Delete:</label>
    <input type="text" id="deleteid" class="input" bind:value={currentid}>

    <button on:click|preventDefault={del} class="button button-delete">Delete User</button>
  </section>

  {#if message}
    <p class="message">{message}</p>
  {/if}
</main>

<style>
  .container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }

  .title {
    text-align: center;
    font-size: 2rem;
    margin-bottom: 20px;
  }

  .section {
    margin-bottom: 20px;
  }

  .subtitle {
    font-size: 1.5rem;
    margin-bottom: 10px;
  }

  .form {
    display: flex;
    flex-direction: column;
  }

  .label {
    margin-bottom: 5px;
  }

  .input {
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 1rem;
  }

  .button {
    padding: 10px;
    font-size: 1rem;
    color: #fff;
    background-color: #007bff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .button:hover {
    background-color: #0056b3;
  }

  .button-delete {
    background-color: #dc3545;
  }

  .button-delete:hover {
    background-color: #c82333;
  }

  .details {
    margin-top: 10px;
  }

  .details h3 {
    margin-bottom: 5px;
  }

  .message {
    color: red;
    font-weight: bold;
  }
</style>
