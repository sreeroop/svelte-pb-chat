<script lang="ts">
    import { currentUser, pb } from "../../lib/pocketbase";

    function logOut() {
        pb.authStore.clear();
    }
</script>

<div
    class="w-full px-12 py-2 flex flex-row justify-between bg-slate-500 fixed top-0 left-0"
>
    <h1>ChatApp</h1>
    {#if $currentUser}
        <button on:click={logOut} class=""> Logout</button>
    {/if}
</div>
