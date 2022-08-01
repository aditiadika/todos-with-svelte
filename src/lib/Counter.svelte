<script>
  let count = 0;
  let text = "";

  const increment = () => {
    count += 1
  }
  const decrement = () => {
    count -= 1
  }
</script>

<p>Count is {count}</p>
{#if count > 5}
  <p>Count is higher</p>
{/if}

<button on:click={increment}>Increment</button>
<button on:click={decrement}>Decrement</button>
