<script>
  import { onMount } from "svelte";
  import { items } from "../store";
  import TodoApi from "../TodoApi";
  import Item from "./Item.svelte";

  function handleNewItem(e) {}

  function handleUpdate(e) {
    const index = $items.findIndex((item) => item.id === e.detail.id);
    $items[index] = e.detail;
    TodoApi.save($items);
  }

  function handleDelete(e) {
    $items = $items.filter((item) => item.id !== e.detail);
    TodoApi.save($items);
  }

  onMount(async () => {
    $items = await TodoApi.getAll();
  });
</script>

<main />

<div class="list">
  {#each $items as item (item)}
    <Item {...item} on:update={handleUpdate} on:delete={handleDelete} />
  {:else}
    <p class="list-status">No Items Exist</p>
  {/each}
</div>

<style>
  .list {
    padding: 15px;
  }
  .list-status {
    margin: 0;
    text-align: center;
    color: #ffffff;
    font-weight: bold;
    font-size: 1.1em;
  }
</style>
