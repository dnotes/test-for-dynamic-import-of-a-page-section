# Original question:

> A simplified analogue of my situation: you have a page called "blog posts", where the posts would be a line of preview cards. When you click a card, the actual blog post content would be loaded and displayed right under the card line. But thats not just an API call to fetch text, i would like to custom build the layout of each post - essentially loading up a mini page within the body of the actual page. And for the URL to also change accordingly, using perhaps the hashtag url idea.
>
> How would i do this with fullstack SvelteKit? Can i build the mini pages as svelte components and dynamically load them in when a card is pressed?

## Test case:

If you really want to load the posts async and show them on the same page, I wonder if it would work to use import.meta.glob, then use {#await} blocks to show them. Something like this:

```
<script>
let cards = getCards() // however you get the cards
let posts = import.meta.glob('src/blog/*.svelte')
</script>

{#each cards as card}
  <h3><a href="#{card.slug}">{card.title}</a></h3>
  {#if $page.url.hash === card.slug}
    {#await posts[`src/blog/${card.slug}.svelte`]() then post}
      <svelte:component this={post} />
    {/await}
  {/if}
{/each}
```

--I'm guessing that something with an async fetch call might be a better option, I'm not sure exactly what this usage of import.meta.glob would do.

## The problem / question:

> For a small number of posts, this will work. For a large number, it will not -- import.meta.glob will basically import every matching file when you render the page, kinda defeating the purpose of the async import in the first place.

Yeah that's what I was worried about, but in that case, I don't understand why import.meta.glob results in async functions to get content instead of the actual content? I mean there's { eager:true } but why even have the option if everything just gets imported anyhow?
