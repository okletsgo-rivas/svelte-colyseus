<script lang="ts">
  import { onMount } from "svelte";
  import { Client, Room } from "colyseus.js";
  export let name: string;

  let messages: string[] = [];

  onMount(async () => {
    const client = new Client("ws://localhost:2567");
    const room: Room<any> = await client.joinOrCreate<any>("chat_room");
    room.state.messages.onAdd = (message, key) => {
      messages = [...messages, message];
      // Scroll to bottom of chat
      // messages.scrollTo(0, messages.scrollHeight);
    };
    room.send("message", "Test");
  });
</script>

<main>
  <h1>Hello {name}!</h1>
  {#each messages as msg}
    <p>{msg}</p>
  {/each}
  <p>
    Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn
    how to build Svelte apps.
  </p>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
