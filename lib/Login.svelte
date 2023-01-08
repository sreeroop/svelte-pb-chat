<script lang="ts">
    import Messages from "./Messages.svelte";
    import { currentUser, pb } from "./pocketbase";

    let username: string;
    let password: string;

    async function login() {
        await pb.collection("users").authWithPassword(username, password);
    }

    async function signUp() {
        try {
            const data = {
                username,
                password,
                passwordConfirm: password,
                name: "hi",
            };
            const createdUser = await pb.collection("users").create(data);
            await login();
        } catch (err) {
            console.log(err);
        }
    }

    function logOut() {
        pb.authStore.clear();
    }
</script>

{#if $currentUser}
    <p>signed in as {$currentUser.username}</p>
    <Messages />
{:else}
    <form on:submit|preventDefault>
        <input placeholder="Username" type="text" bind:value={username} />
        <input placeholder="Passeword" type="text" bind:value={password} />
        <button on:click={signUp}>Sign Up</button>
        <button on:click={login}>Login</button>
    </form>
{/if}
