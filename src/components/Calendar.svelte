<script>
  import _ from 'lodash';
  import CalendarCell from './CalendarCell.svelte';
  import service from '../service';

  let hours = _.range(24);
  let days = _.range(1, 32).map((day) => ("Oct " + day));

  let isLoaded = false;

  function load(){
    isLoaded = true;
  }

  function searchAll() {
    service.searchAllCells();
  }
</script>

<style>
  table {
    border-right: 2px solid gray;
    border-spacing: 0;
    border-collapse: collapse;
  }
  .day-header {
    border: 1px solid gray;
    white-space: nowrap
  }
  .day-header:hover {
    cursor: pointer;
  }
  .hour-cell:hover {
    background-color: #559 !important;
    color: white;
  }
  .hour-cell {
    width: 120px;
    text-align: center;
    border-left: 1px solid gray;
    border-bottom: 1px solid #eee;
    cursor: pointer;
    height: 30px;
    padding: 0;
  }
  .hour-cell > div {
    height: 100%;
  }
  .hour-cell .time {
    padding-top: 10px; /* poor man's vertical centering */
  }
  .hour-cell .searching {
    color: blue;
    font-size: 12px;
    padding-top: 12px; /* poor man's vertical centering */
  }
  .hour-cell .good-results {
    color: #090;
    background: #efffef;
  }
  .hour-cell .weak-results {
    color: orange;
    background: #ffffef;
  }
  .hour-cell .bad-results {
    color: red;
    background: #ffefef;
  }

  .btn {
    border-radius: 10px;
    background: purple;
    color: #eee;
  }
</style>

<div class="calendar">
  {#if !isLoaded}
    <button class="btn" on:click={load}>Load</button>
  {:else}
    <button class="btn" on:click={searchAll}>Search all month</button>

    <table>
      <tr>
        {#each days as day (day)}
          <th class="day-header">
            {day}
          </th>
        {/each}
      </tr>
      {#each hours as hour (hour)}
        <tr>
          {#each days as day (day)}
            <td class="hour-cell">
              <CalendarCell day={day} hour={hour} />
            </td>
          {/each}
        </tr>
      {/each}
    </table>
  {/if}
</div>
