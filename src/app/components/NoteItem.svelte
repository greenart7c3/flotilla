<script lang="ts">
  import type {NativeEmoji} from "emoji-picker-element/shared"
  import type {TrustedEvent} from "@welshman/util"
  import {pubkey} from "@welshman/app"
  import Icon from "@lib/components/Icon.svelte"
  import EmojiButton from "@lib/components/EmojiButton.svelte"
  import Content from "@app/components/Content.svelte"
  import NoteCard from "@app/components/NoteCard.svelte"
  import ReactionSummary from "@app/components/ReactionSummary.svelte"
  import {publishDelete, publishReaction} from "@app/commands"

  export let url
  export let event

  const onReactionClick = (content: string, events: TrustedEvent[]) => {
    const reaction = events.find(e => e.pubkey === $pubkey)

    if (reaction) {
      publishDelete({relays: [url], event: reaction})
    } else {
      publishReaction({event, content, relays: [url]})
    }
  }

  const onEmoji = (emoji: NativeEmoji) =>
    publishReaction({event, relays: [url], content: emoji.unicode})
</script>

<NoteCard {event} class="card2 bg-alt">
  <Content {event} expandMode="inline" />
  <div class="flex w-full justify-between gap-2">
    <ReactionSummary relays={[url]} {event} {onReactionClick}>
      <EmojiButton {onEmoji} class="btn btn-neutral btn-xs h-[26px] rounded-box">
        <Icon icon="smile-circle" size={4} />
      </EmojiButton>
    </ReactionSummary>
  </div>
</NoteCard>
