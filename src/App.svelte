<script lang="ts">
  import Login from "./lib/Login.svelte";
  import { currentUser } from "../lib/pocketbase";
  import Messages from "./lib/Messages.svelte";
</script>

<h1 class="text-3xl font-bold underline">Hello world!</h1>

{#if $currentUser}
  <p>signed in as {$currentUser.username}</p>
  <Messages />
{:else}
  <Login />
{/if}

<style>
</style>
