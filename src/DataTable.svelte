<script lang="ts">
  import jsonData from "./lib/17-sep.json";
  import "./app.css";
  let searchQuery = "";
  let sortByCoursesCompleted = false;
  let completions = 0;

  $: sortedData = [...jsonData];

  function sortByCourses() {
    if (sortByCoursesCompleted) {
      sortedData = [...jsonData];
    } else {
      sortedData = [...jsonData].sort(
        (a, b) => b["# of Courses Completed"] - a["# of Courses Completed"]
      );
    }
    sortByCoursesCompleted = !sortByCoursesCompleted;
  }

  function calcCompletions() {
    jsonData.forEach((entry) => {
      if (entry["Total Completions of both Pathways"] === "Yes") {
        completions++;
      }
    });
    return completions;
  }

  $: filteredData = sortedData
    .filter((entry) => entry["Total Completions of both Pathways"] === "Yes")
    .concat(
      sortedData.filter(
        (entry) => entry["Total Completions of both Pathways"] !== "Yes"
      )
    )
    .filter((entry) =>
      entry["Student Name"].toLowerCase().includes(searchQuery.toLowerCase())
    );
</script>

<main class="flex flex-col gap-10 justify-center items-center">
  <div
    class="min-h-fit min-w-fit border border-green-400 bg-green-100 rounded-xl text-center p-4 text-base font-semibold text-slate-500"
  >
    No. of Total Completions : {calcCompletions()}
    <i class="fa-solid fa-gift text-green-600 pl-2" />
  </div>

  <input
    type="text"
    placeholder="Search by Student Name"
    bind:value={searchQuery}
    class="px-4 py-2 border-2 border-green-300 rounded-lg w-[55%] focus:outline-none focus:ring-2 focus:ring-green-400 focus:border-transparent focus:shadow-lg focus:transition-all duration-500 ease-in-out"
  />

  <div class="table-wrapper flex">
    <table class="min-w-[85%]">
      <thead class="text-center">
        <tr>
          <th class="px-6 py-3 bg-gray-200 font-semibold w-[27%] rounded-ss-xl"
            >Student Name</th
          >
          <th class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold w-[15%]"
            >Completion Status</th
          >
          <th class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold w-[15%]"
            >Redemption Status</th
          >
          <th class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold w-[15%]"
            ><button on:click={sortByCourses}>
              {#if sortByCoursesCompleted}
                <span class="mr-2">Course Badges</span><i
                  class="fa-solid fa-sort"
                />
              {:else}
                <span class="mr-2">Course Badges</span><i
                  class="fa-solid fa-sort text-gray-400"
                />
              {/if}
            </button>
          </th>
          <th class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold w-[14%]"
            >Skill Badges</th
          >
          <th
            class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold w-[14%] rounded-se-xl"
            >GenAI Game</th
          >
        </tr>
      </thead>
      <tbody class="border border-gray-100">
        {#each filteredData as entry, index (entry["Google Cloud Skills Boost Profile URL"])}
          <tr class={index % 2 === 0 ? "bg-gray-100" : "bg-white"}>
            <td class="px-6 py-4 w-[27%] tracking-wide">
              <a
                href={entry["Google Cloud Skills Boost Profile URL"]}
                target="_blank"
              >
                {entry["Student Name"]}
                {#if entry["Total Completions of both Pathways"] == "Yes"}
                  <i class="fa-solid fa-medal text-yellow-400" />
                {/if}
              </a>
            </td>
            <td class="px-6 py-4 w-[15%] text-center">
              {#if entry["Total Completions of both Pathways"] === "Yes"}
                <i class="fas fa-circle-check text-green-500" />
              {:else}
                <i class="fas fa-circle-xmark text-red-500" />
              {/if}
            </td>
            <td class="px-6 py-4 w-[15%] text-center">
              {#if entry["Redemption Status"] === "Yes"}
                <i class="fas fa-circle-check text-green-500" />
              {:else}
                <i class="fas fa-circle-xmark text-red-500" />
              {/if}
            </td>
            <td class="px-6 py-4 w-[15%] text-center"
              >{entry["# of Courses Completed"]}</td
            >
            <td class="px-6 py-4 w-[14%] text-center"
              >{entry["# of Skill Badges Completed"]}</td
            >
            <td class="px-6 py-4 w-[14%] text-center"
              >{entry["# of GenAI Game Completed"]}</td
            >
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</main>

<style>
  .table-wrapper {
    min-width: 85%;
    place-content: center;
  }
</style>
