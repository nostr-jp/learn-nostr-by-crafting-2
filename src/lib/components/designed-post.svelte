<script lang="ts">
  import type { Nostr } from "nosvelte";
  import { format, fromUnixTime } from "date-fns";
  export let metadata: Nostr.Event<Nostr.Kind.Metadata>;
  export let event: Nostr.Event<Nostr.Kind.Text>;
</script>

<div class="d-flex p-2">
  <div class="user_icon flex-shrink-0">
    <img
      src={JSON.parse(metadata.content).picture}
      class="img-fluid"
      alt=""
      srcset=""
    />
  </div>
  <div class="ms-2 flex-fill">
    <div class="d-flex">
      <div class="flex-fill">
        {JSON.parse(metadata.content).display_name ??
          JSON.parse(metadata.content).DisplayName}
        <span class="text-secondary ms-2">
          @{JSON.parse(metadata.content).name ?? "nostrich"}
        </span>
      </div>
      <div class="flex-shrink-0">
        {format(fromUnixTime(event.created_at), "y/M/d h:m:ss")}
      </div>
    </div>
    <div class="text-break lf">
      {event.content}
    </div>
  </div>
</div>

<style>
.user_icon {
  width: 48px;
  height: 48px;
  aspect-ratio: 1;
  border-radius: 24px;
  overflow: hidden;
}
</style>
