<script lang="ts">
  import { onMount } from "svelte";
  import { Client, Room } from "colyseus.js";

  let messages: string[] = [];

  onMount(async () => {
    const client = new Client("ws://localhost:2567");
    const room: Room<any> = await client.joinOrCreate<any>("chat_room");
    room.state.messages.onAdd = (message: string) => {
      messages = [...messages, message];
    };
    room.send("message", "Test");
  });
</script>

<main>
  <h1>Hello World!</h1>
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
