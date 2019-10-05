<script>
  function randomMilliseconds () {
    return Math.floor(Math.random() * 500);
  }

  import service from '../service';

  export let day, hour;

  $: isSearching = false;
  $: searchResults = {
    options: null
  };

  function cellClicked() {
    let alreadySearching = isSearching;

    searchResults.options = null;
    isSearching = !alreadySearching;

    if (!alreadySearching) {
      // Simulate an AJAX request:
      isSearching = true;

      setTimeout(() => {
        isSearching = false;
        searchResults.options = Math.floor(Math.random() * 5);
      }, randomMilliseconds());
    }
  }

  service.addCell({cellClicked});

  function getClass(data){
    if (data > 3) {
      return 'goodresults'
    }

    if (data >= 1 && data <= 3) {
      return 'weakresults'
    }

    if (data == 0) {
      return 'badresults'
    }
  }
</script>

<style>
  .searching {
    color: blue;
    font-size: 10px;
    padding-top: 6px; /* poor man's vertical centering */
  }
  .goodresults {
    color: #090;
    background: #efffef;
  }
  .result-label {
    font-size: 8px;
  }
  .weakresults {
    color: orange;
    background: #ffffef;
  }
  .badresults {
    color: red;
    background: #ffefef;
  }
  :hover {
    background-color: #559 !important;
    color: white;
  }
</style>

<!-- view -->
<div class={getClass(searchResults.options)}>

  {#if isSearching}

    <div>...</div>

  {:else if !isSearching && searchResults.options === null}

    <div>{hour}:00</div>

  {:else if !isSearching && searchResults.options !== null}

    <div v-if="showSearchResults()">
      <div>{searchResults.options}</div>
      <div class="result-label">results</div>
    </div>

  {/if}
</div>