<script lang="ts">
  import { page } from "$app/stores";
  import { getTable } from "nostr-key-value";

  let tableName = $page.url.searchParams.get("table") ?? "";
  // "nostr-arrival-rate_kirino";
  let authorHex = $page.url.searchParams.get("hex") ?? "";
  // "a3c13ef4c9eccfde01bd9326a2ab08b2ad7dc57f3b77db77723f8e2ad7ba24d6";
  let relays = ($page.url.searchParams.get("relays") ?? "wss://relay-jp.nostr.wirednet.jp").replace(",", "\n")
  let result = "";
  const getDatabase = async () => {
    result = "processing...";
    const table = await getTable(relays.split("\n"), authorHex, tableName);
    console.log(table);
    if (!table) {
      result = "none";
    } else {
      result = JSON.stringify(table);
    }
  };
</script>

<!-- {#if items} -->
<div class="bg-main">
  <div
    class="max-width mx-auto py-2 align-items-center justify-content-between d-flex"
  >
    <div class="fs-6">nostr-key-value viewer</div>
    <div class="fs-6">github</div>
  </div>
</div>
<div class="max-width mx-auto py-4">
  <div class="mb-3">
    <label for="" class="form-label">table name:</label>
    <input type="text" class="form-control" value={tableName} />
  </div>
  <div class="mb-3">
    <label for="" class="form-label">author hex:</label>
    <input type="text" class="form-control" value={authorHex} />
  </div>
  <div class="mb-3">
    <label for="" class="form-label">relays:</label>
    <textarea class="form-control" value={relays} />
  </div>
  <div class="mt-2 text-center">
    <button class="btn btn-primary" on:click={getDatabase}>Get Table</button>
  </div>
  <div class="mt-4"><label for="" class="form-label">result</label></div>
  <textarea class="form-control result" value={result} />
</div>

<style>
  .result {
    height: 300px;
    font-size: 12px;
    font-family: sans-serif;
  }
</style>
