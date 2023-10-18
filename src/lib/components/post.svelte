<script lang="ts">
  import type { Nostr } from "nosvelte";
  import { format, fromUnixTime } from "date-fns";
  export let metadata: Nostr.Event<Nostr.Kind.Metadata>;
  export let event: Nostr.Event<Nostr.Kind.Text>;
</script>

<!-- 演習3: 必要なデータ一覧を表示する -->
<img
  src={JSON.parse(metadata.content).picture}
  class="user_icon"
  alt=""
/>
<div>ユーザー名：{JSON.parse(metadata.content).display_name ?? JSON.parse(metadata.content).DisplayName}
</div>
<div>アカウント：@{JSON.parse(metadata.content).name ?? "undefined"}</div>
<div>投稿時刻：{event.created_at}</div>
<div>投稿時刻：{format(fromUnixTime(event.created_at), "y/M/d h:m:ss")}</div>
<div>投稿内容：{event.content}</div>

<style>
.user_icon {
  width: 48px;
  aspect-ratio: 1;
  object-fit: cover;
}
</style>
