<script>
  import { pageItem, page, refreshNotebook } from "../../stores";
  import { getToken } from "../../getToken";
  import IcoGoBack from "../img/IcoGoBack.svelte";
  import PageIcoOutline from "../img/PageIcoOutline.svelte";
  import PageIcoMindmap from "../img/PageIcoMindmap.svelte";
  import PageIcoGrouping from "../img/PageIcoGrouping.svelte";
  import PageTitle from "./Tools/PageTitle.svelte";
  import PageTypeCard from "./Tools/PageTypeCard.svelte";
  import Loader from "../Loader.svelte";
  import { goto } from "@sapper/app";
  import DeletionModalPage from "./Tools/DeletionModalPage.svelte";

  $: pageInfo = $pageItem;
  let activeModal = false;
  const pageTypes = ["outline", "mindmap", "grouping"];

  async function remove() {
    let notebook = pageInfo.notebook.id;

    try {
      await fetch(`/api/pages/${$page.params.pageId}`, {
        method: "DELETE",
        credentials: "include",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
          "csrf-token": getToken(),
        },
      }).then(() => {
        $refreshNotebook = { id: notebook, time: Date.now() };
        goto(`notebooks/${pageInfo.notebook.shortId}`);
      });
    } catch (err) {
      console.error(err);
    }
  }
</script>

<style>
  :global(.pageWorkspace .content.unfinished) {
    grid-column: inherit;
    background: #f9fbfc;
  }
  .breadcrumbs {
    text-decoration: none;
    display: grid;
    grid-template-columns: max-content 1fr;
    grid-gap: 5px;
    position: fixed;
    top: 20px;
    left: calc(var(--base) * 2);
  }
  .breadcrumbs h5 {
    color: #888;
    font-weight: 500;
    margin: 0;
    margin-bottom: 0;
  }
  .Page {
    width: 100%;
    display: grid;
    grid-column: inherit;
    gap: 70px;
    position: absolute;
    top: 50%;
    left: 50%;
    padding: 40px;
    text-align: center;
    transform: translate(-50%, -50%);
  }
  .PageType {
    display: grid;
    gap: 100px;
    justify-content: center;
    grid-template-columns: repeat(3, 200px);
  }
  @media (min-width: 641px) and (max-width: 920px) {
    :global(.pageWorkspace .content.unfinished) {
      height: 100vh;
    }
    .PageType {
      gap: 30px;
    }
    .PageType {
      grid-template-columns: repeat(3, 1fr);
    }
  }
  @media (max-width: 640px) {
    .breadcrumbs {
      position: inherit;
    }
    .Page {
      gap: 30px;
      left: inherit;
      top: inherit;
      display: grid;
      grid-column: inherit;
      position: inherit;
      transform: translate(0, 0);
    }
    .PageType {
      gap: 20px;
      grid-template-columns: 1fr;
    }
  }
</style>

{#if pageInfo.type === 'loading'}
  <Loader />
{:else}
  <a href={`notebooks/${pageInfo.notebook.shortId}`} class="breadcrumbs">
    <IcoGoBack />
    <h5>{pageInfo.notebook.name}</h5>
  </a>
  <div class="Page">
    <PageTitle {pageInfo} bind:activeModal />
    <div class="PageType">
      {#each pageTypes as type}
        <PageTypeCard
          {type}
          {pageInfo}
          context={pageInfo.noteContexts.find((item) => item.type === type)} />
      {/each}
    </div>
  </div>
{/if}
{#if activeModal}
  <DeletionModalPage {remove} bind:activeModal {pageInfo} />
{/if}
