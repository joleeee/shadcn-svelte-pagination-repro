<script>
  import Button from '$lib/registry/ui/button/button.svelte';
  import SidebarInset from '$lib/registry/ui/sidebar/sidebar-inset.svelte';
  let fixed = $state(false);
  let pagination = $state(true);
  let page = $state(0);
  const labels = ['first', 'previous', 'next', 'last'];
  const icons = ['«', '‹', '›', '»'];
  const targets = () => [0, Math.max(0, page - 1), Math.min(2, page + 1), 2];
</script>

<SidebarInset>
  <header>
    <label><input type="checkbox" bind:checked={pagination} /> Pagination</label>
    <label><input type="checkbox" bind:checked={fixed} /> Add position: relative</label>
  </header>
  <section aria-label="Scrollable rows">
    {#each Array.from({ length: page === 2 ? 13 : 25 }) as _, i}
      <div class="row">Row {page * 25 + i + 1}</div>
    {/each}
    {#if pagination}
      <footer class:relative={fixed}>
        <span>Page {page + 1} of 3</span>
        <div>
          {#each labels as label, i}
            <Button variant="outline" size="icon"
              disabled={i < 2 ? page === 0 : page === 2}
              onclick={() => page = targets()[i]}
            >
              <span class="sr-only">Go to {label} page</span>{icons[i]}
            </Button>
          {/each}
        </div>
      </footer>
    {/if}
  </section>
</SidebarInset>

<style>
  header { height: 64px; display: flex; align-items: center; gap: 24px; padding: 0 16px; }
  section { height: calc(100svh - 64px); overflow-y: auto; padding: 16px; box-sizing: border-box; }
  .row { height: 48px; display: flex; align-items: center; border-bottom: 1px solid #ddd; box-sizing: border-box; }
  footer { display: flex; justify-content: space-between; align-items: center; margin-top: 16px; }
</style>
