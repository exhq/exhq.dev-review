<script lang="ts">
import { onMount } from 'svelte';
let token:string|null = null
onMount(() => {
  const searchParams = new URLSearchParams(window.location.search);
  token = searchParams.get("access_token")
});



async function sendReview(review:string){
  const url = new URL("https://review.exhq.dev/sendreview");
  url.searchParams.append("review", review);
  const authHeader = token;
  if (authHeader == null){
    return false;
  }
  const reviewBody = `review=${review}`; // Encode review string

  try {
    const response = await fetch(url, {
      method: "POST",
      headers: {
        "Auth": authHeader,
      },
      body: reviewBody,
    });

    if (!response.ok) {
      alert("somethniung went wrong, im too lazy to tell you what went wrong, but something defenitely did go wrong")
      return
    }
    
    const data = await response.text();
    console.log("Response:", data);
  } catch (error) {
    //no
  }
  alert("revoiew sent successfly")
}

</script>

<main>

{#if token === null}
  <p>stupid bitchass trying to send a review without logging in</p>
{:else}
  <p>im too lazy to make this page look good</p>
  <textarea id="balls" placeholder="put yo shit here" on:keypress={(input)=> {
    if(input.key == "Enter"){
      input.preventDefault()
      const shit = document.getElementById("balls");
      //just to make ts shut the fuck up
      if (shit instanceof HTMLTextAreaElement) {
        const content = shit.value
        console.log("Textarea content:", content);
        sendReview(content)
      } else {
        alert("you fucking broke ts, how")
      }
    }
  }} cols="30" rows="10"></textarea>
{/if}


  <!-- <h1>Vite + Svelte</h1>

  <div class="card">
    
  </div>

  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme" target="_blank" rel="noreferrer">SvelteKit</a>, the official Svelte app framework powered by Vite!
  </p>

  <p class="read-the-docs">
    Click on the Vite and Svelte logos to learn more
  </p> -->
</main>

<style>

</style>
