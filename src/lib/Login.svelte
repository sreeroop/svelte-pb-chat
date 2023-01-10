<script lang="ts">
    import { pb } from "../../lib/pocketbase";

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

<div class="md:w-8/12 lg:w-8/12 lg:ml-20">
    <form on:submit|preventDefault>
        <!-- Email input -->
        <div class="mb-6">
            <input
                bind:value={username}
                type="text"
                class="form-control block w-full px-4 py-2 text-xl font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                placeholder="Email address"
            />
        </div>

        <!-- Password input -->
        <div class="mb-6">
            <input
                bind:value={password}
                type="password"
                class="form-control block w-full px-4 py-2 text-xl font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                placeholder="Password"
            />
        </div>
        <!-- Submit button -->
        <button
            on:click={signUp}
            type="submit"
            class="inline-block px-7 py-3 mt-2 bg-blue-600 text-white font-medium text-sm leading-snug uppercase rounded shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out w-full"
            data-mdb-ripple="true"
            data-mdb-ripple-color="light"
        >
            Sign up
        </button>
        <button
            on:click={login}
            type="submit"
            class="inline-block px-7 py-3 mt-2 bg-blue-600 text-white font-medium text-sm leading-snug uppercase rounded shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out w-full"
            data-mdb-ripple="true"
            data-mdb-ripple-color="light"
        >
            Login
        </button>
    </form>
</div>
