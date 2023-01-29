<script lang="ts">
  let posts = Object.fromEntries(
    Object.entries(import.meta.glob('./blog/**/+page.svelte'))
    .map(([path, fn]) => { return [
      path.replace(/.+\/blog\//, '').replace('\/+page.svelte', ''),
      fn
    ]})
  )
  import { page } from '$app/stores'
</script>

<p>Test for <a href="https://discord.com/channels/457912077277855764/1069299168402743387">this Discord post</a></p>

{#each Object.entries(posts) as [slug,post]}
  <h3><a href="#{slug}">{slug}</a></h3>
  {#if $page.url.hash === `#${slug}`}
    {#await posts[slug]() then post}
      <svelte:component this={post.default} />
    {/await}
  {/if}
{/each}