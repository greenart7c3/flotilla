<script lang="ts">
  import type {NativeEmoji} from "emoji-picker-element/shared"
  import EmojiButton from "@lib/components/EmojiButton.svelte"
  import Icon from "@lib/components/Icon.svelte"
  import {tagRoom} from "@app/state"
  import {publishReaction} from "@app/commands"

  export let url, room, event

  const onEmoji = (emoji: NativeEmoji) =>
    publishReaction({
      event,
      relays: [url],
      content: emoji.unicode,
      tags: [tagRoom(room, url)],
    })
</script>

<EmojiButton {onEmoji} class="btn join-item btn-xs">
  <Icon icon="smile-circle" size={4} />
</EmojiButton>
