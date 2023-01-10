<script lang="ts">
    import { onMount, onDestroy } from "svelte";
    import { currentUser, pb } from "../../lib/pocketbase";

    let messages = [];
    let newMessage: string;
    let unsubscribe: () => void;
    onMount(async () => {
        const resultList = await pb.collection("messages").getList(1, 50, {
            sort: "created",
            expand: "user",
        });

        messages = resultList.items;

        unsubscribe = await pb
            .collection("messages")
            .subscribe("*", async ({ action, record }) => {
                if (action === "create") {
                    const user = await pb
                        .collection("users")
                        .getOne(record.user);
                    record.expand = { user };
                    messages = [...messages, record];
                }
                if (action === "delete") {
                    messages = messages.filter((m) => m.id !== record.id);
                }
            });
    });

    onDestroy(() => {
        unsubscribe();
    });
    async function sendMessage() {
        const data = {
            message: newMessage,
            user: $currentUser.id,
        };
        const createdMessage = await pb.collection("messages").create(data);
    }
</script>

<!-- 
<div class="messages">
    {#each messages as message (message.id)}
        <div class="msg">
            <img
                src={`https://avatars.dicebear.com/api/identicon/${message.expand?.user?.username}.svg`}
                width="40px"
                alt="avatar"
                class="avatar"
            />
            <div>
                <small>
                    Sent by @{message.expand?.user?.username}
                </small>
                <p class="msg-text">{message?.message}</p>
            </div>
        </div>
    {/each}
</div> -->

<div class="w-full px-5 flex flex-col justify-between mb-20">
    <div class="flex flex-col mt-5">
        {#each messages as message (message.id)}
            <div
                class={`flex 
                mb-4 justify-end `}
                class:flex-row-reverse={message.expand?.user?.username !==
                    $currentUser.username}
            >
                <div
                    class="mr-2 ml-2 py-3 px-4 bg-green-700 rounded-xl min-w-1/2 text-white"
                >
                    <p>{message?.message}</p>
                    <small>
                        @{message.expand?.user?.username}
                    </small>
                </div>
                <img
                    src={`https://avatars.dicebear.com/api/identicon/${message.expand?.user?.username}.svg`}
                    class="object-cover h-8 w-8"
                    alt=""
                    width="40px"
                />
            </div>
        {/each}
    </div>
</div>

<form
    on:submit|preventDefault={sendMessage}
    class="flex flex-row justify-center items-center fixed bottom-0 w-full px-6 py-2 bg-slate-600"
>
    <div class="py-1">
        <input
            class="w-full bg-gray-300 py-5 px-3 rounded-xl border-red-100"
            type="text"
            placeholder="type your message here..."
            bind:value={newMessage}
        />
    </div>
    <button type="submit" class="px-12 py-2 h-9">Send</button>
</form>
