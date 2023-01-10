<script lang="ts">
  import Login from "./lib/Login.svelte";
  import { currentUser } from "../lib/pocketbase";
  import Messages from "./lib/Messages.svelte";
  import Nav from "./lib/Nav.svelte";
</script>

<Nav />

{#if $currentUser}
  <p>signed in as {$currentUser.username}</p>
  <Messages />
{:else}
  <Login />
{/if}

<style>
</style>
