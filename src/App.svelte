<script lang="ts">
  import DataTable from "./DataTable.svelte";
  import "./app.css";
</script>

<main class="apply-font">
  <div class="flex flex-col justify-center items-center">
    <div class="flex justify-center py-6">
      <img src="gdsc-main-bg.png" alt="jam_image" class="w-[80%] md:w-[60%]" />
    </div>
  </div>
  <DataTable />
</main>

<style>
  .apply-font {
    font-family: "Poppins", sans-serif;
  }
</style>
