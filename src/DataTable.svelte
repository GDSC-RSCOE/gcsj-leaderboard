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

<main class="flex flex-col gap-6 md:gap-10 justify-center items-center">
  <div
    class="min-h-fit min-w-fit border border-green-400 bg-green-100 rounded-xl text-center md:p-4 p-3 md:text-base text-sm font-semibold text-slate-500"
  >
    No. of Total Completions : {calcCompletions()}
    <i class="fa-solid fa-gift text-green-600 pl-2" />
  </div>

  <input
    type="text"
    placeholder="Search by Student Name"
    bind:value={searchQuery}
    class="px-4 md:py-2 py-1 border-2 border-green-300 rounded-lg md:w-[55%] w-[50%] focus:outline-none focus:ring-2 focus:ring-green-400 focus:border-transparent focus:shadow-lg focus:transition-all duration-500 ease-in-out"
  />

  <div class="table-wrapper flex">
    <table class="min-w-[85%] md:text-base text-sm">
      <thead class="text-center">
        <tr>
          <th
            class="px-6 py-3 bg-gray-200 font-semibold md:w-[27%] w-[65%] rounded-ss-xl"
            >Student Name</th
          >
          <th
            class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold md:w-[15%] w-[35%] md:rounded-none rounded-se-xl"
            >Completion Status</th
          >
          <th
            class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold w-[15%] md:table-cell hidden"
            >Redemption Status</th
          >
          <th
            class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold w-[15%] md:table-cell hidden"
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
          <th
            class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold w-[14%] md:table-cell hidden"
            >Skill Badges</th
          >
          <th
            class="px-6 py-3 bg-gray-200 text-gray-600 font-semibold w-[14%] rounded-se-xl md:table-cell hidden"
            >GenAI Game</th
          >
        </tr>
      </thead>
      <tbody class="border border-gray-100">
        {#each filteredData as entry, index (entry["Google Cloud Skills Boost Profile URL"])}
          <tr class={index % 2 === 0 ? "bg-gray-100" : "bg-white"}>
            <td class="px-6 py-4 md:w-[27%] w-[65%] tracking-wide">
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
            <td class="px-6 py-4 md:w-[15%] w-[35%] text-center">
              {#if entry["Total Completions of both Pathways"] === "Yes"}
                <i class="fas fa-circle-check text-green-500" />
              {:else}
                <i class="fas fa-circle-xmark text-red-500" />
              {/if}
            </td>
            <td class="px-6 py-4 w-[15%] text-center md:table-cell hidden">
              {#if entry["Redemption Status"] === "Yes"}
                <i class="fas fa-circle-check text-green-500" />
              {:else}
                <i class="fas fa-circle-xmark text-red-500" />
              {/if}
            </td>
            <td class="px-6 py-4 w-[15%] text-center md:table-cell hidden"
              >{entry["# of Courses Completed"]}</td
            >
            <td class="px-6 py-4 w-[14%] text-center md:table-cell hidden"
              >{entry["# of Skill Badges Completed"]}</td
            >
            <td class="px-6 py-4 w-[14%] text-center md:table-cell hidden"
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
