<script lang="ts">
    import Header from "./lib/Header.svelte";
    import {page, pollDTO} from "./store";
    import Poll from './view/Poll.svelte'
    import Footer from "./lib/Footer.svelte";
    import NotificationArea from "./lib/NotificationArea.svelte";

    $: { //TODO: find a better way to handle this
        if ($pollDTO !== undefined) {
            page.set(Poll)
        }
    }

    if (window.location.search.includes("?k=")) {
        page.set(Poll)
    }

    function showReviewHint(): boolean {
        return process.env.NODE_ENV === "review"
    }

    function showStagingHint(): boolean {
        return process.env.NODE_ENV === "staging"

    }

</script>
<div class="flex flex-col min-h-screen">
    <Header/>
    {#if showReviewHint()}
        <div class="w-full h-auto dark:text-black dark:bg-yellow-600 mx-auto text-center h-9 bg-yellow-400 p-1 text-lg">This
            instance is only for reviewing and
            tests. Polls are not synchronised with other browsers!<a
                    class="text-blue-900 pl-3" href="https://tetrisiq.github.io/poll-dapp/">Use the Live instance</a>
        </div>
    {/if}
    {#if showStagingHint()}
        <div class="w-full h-auto dark:text-black dark:bg-yellow-600 mx-auto text-center h-9 bg-yellow-400 p-1 text-lg">You are
            on the staging instance. The Relay server loses all information after 15 minutes!<a
                    class="text-blue-900 pl-3" href="https://tetrisiq.github.io/poll-dapp/">Use the Live instance</a>
        </div>
    {/if}
    <main class="mx-auto flex-grow w-full xl:px-40 l:px-14 text-center">
        <NotificationArea/>
        <svelte:component this={$page}/>
    </main>
    <Footer/>
</div>

