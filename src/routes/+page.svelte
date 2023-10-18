<script lang="ts">
  import { NostrApp, UniqueEventList, Metadata, Nostr } from "nosvelte";
  import { createRxForwardReq } from "rx-nostr";
  import "websocket-polyfill";

  // components
  import Post from "$lib/components/post.svelte";
  import DesignedPost from "$lib/components/designed-post.svelte";

  const req = createRxForwardReq();

  // 演習 1: リレーの URL を指定してみましょう
  //   以下の内容は配列になっています。複数のリレーを設定することができます。
  //   配列の中身は、カンマで区切って以下のように書きます。
  //   ["wss://your-relay-url1", "wss://your-relay-url2", "wss://your-relay-url3"];
  const relays: string[] = [???];

  // 取得したイベントを時系列で並べ替える
  const sorted = (events: Nostr.Event[]) => {
    return [...events].sort((a, b) => b.created_at - a.created_at);
  };
</script>

<NostrApp {relays}>
  <UniqueEventList
    queryKey={["timeline", "feed"]}
    filters={[
      {
        kinds: [1],
        limit: 100,
      },
    ]}
    {req}
    let:events
  >
    <div slot="loading">
      <p>Loading...</p>
    </div>

    <div slot="error" let:error>
      <p>{error}</p>
    </div>

    <div>
      {#each sorted(events) as event (event.id)}
        <section>
          <Metadata
            queryKey={["timeline", "metadata", event.pubkey]}
            pubkey={event.pubkey}
            let:metadata
          >
            {JSON.stringify(event)}
            {JSON.stringify(metadata)}

            <!-- 演習2: 投稿をコンポーネントに表示してみよう -->
            <!-- <Post metadata={???} event={???}></Post> -->

            <!-- 演習3: レイアウトを整えてクライアントっぽくする -->
            <!-- <DesignedPost metadata={???} event={???}></DesignedPost> -->
          </Metadata>
        </section>
      {/each}
    </div>
  </UniqueEventList>
</NostrApp>
