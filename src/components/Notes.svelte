<script>
  import NewNote from "./notes/NewNote.svelte";
  import NotesList from "./notes/NotesList.svelte";
  import NoteEdit from "./notes/NoteEdit.svelte";
  import IcoGoBack from "./img/IcoGoBack.svelte";
  import NavNotes from "./img/NavNotes.svelte";
  export let id = false;
</script>

<style>
  .Toolbar {
    display: flex;
    top: 0;
    position: relative;
    justify-content: space-between;
    background-color: #ffffff;
    z-index: 3;
    padding: 20px calc(var(--base) * 2) 0;
  }
  nav.Toolbar::before {
    content: "";
    background: #ffffff;
    width: 80px;
    height: 130px;
    position: absolute;
    top: 0;
    left: -80px;
  }
  .Toolbar section {
    display: flex;
    flex-direction: column;
    margin: 0;
  }
  .Toolbar section * {
    float: left;
  }
  .breadcrumbs {
    margin-bottom: 10px;
  }
  .breadcrumbs h5 {
    color: #888888;
    font-weight: 500;
    margin: 0;
  }
  .library :global(svg) {
    width: 16px;
    margin-right: 5px;
    float: left;
  }
  .library h3 {
    font-weight: 600;
    margin: 0;
  }
  @media (max-width: 720px) {
    nav.Toolbar::before {
      content: none;
    }
    .Toolbar {
      position: relative;
      padding: calc(var(--base) * 1.5) calc(var(--base) * 2);
    }
    .Toolbar:first-child:nth-last-child(5) :global(.new-button) {
      display: none;
    }
    .Toolbar section {
      display: table;
      width: 100%;
      text-align: center;
    }
    .breadcrumbs {
      margin-bottom: 0;
      margin-top: 3px;
    }
    .breadcrumbs h5 {
      display: none;
    }
    .library {
      float: inherit !important;
      display: inline-table;
    }
    .NotesEditor.noteColumn {
      grid-template-columns: 1fr;
    }
    .noteColumn .notesList {
      display: none;
    }
  }

  .Body {
    display: grid;
    grid-gap: 1rem;
  }
  .NotesEditor {
    grid-template-columns: 300px 1fr;
    grid-gap: 0;
  }
  .NotesEditor :global(.CardHeader) {
    display: none;
  }
  .NotesEditor :global(.Cards) {
    padding: 20px 20px 0 20px;
    height: 100vh;
    overflow-y: scroll;
    position: sticky;
    top: 0;
  }
  .NotesEditor :global(.Cards:last-child::after) {
    content: "";
    background: transparent;
    width: 100%;
    height: 1px;
    float: left;
  }
</style>

<!--<WorkspaceMenu {workspace} />-->
{#if !id}
  <nav class="Toolbar">
    <section>
      <a href="/" class="breadcrumbs">
        <IcoGoBack />
        <h5>Home</h5>
      </a>
      <div class="library">
        <NavNotes />
        <h3>Notes library</h3>
      </div>
    </section>
    <NewNote ntbkClose={false} />
  </nav>
{/if}
<div class="Body {id ? 'noteColumn' : ''}" class:NotesEditor={id}>
  <div class="notesList">
    <NotesList />
  </div>
  {#if id}
    <NoteEdit />
  {/if}
</div>
