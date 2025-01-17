<script lang="ts">
  import {onMount} from "svelte"
  import type {Readable} from "svelte/store"
  import {createEditor, type Editor, EditorContent} from "svelte-tiptap"
  import {append} from "@welshman/lib"
  import {isMobile} from "@lib/html"
  import {fly, slideAndFade} from "@lib/transition"
  import Icon from "@lib/components/Icon.svelte"
  import Button from "@lib/components/Button.svelte"
  import ModalFooter from "@lib/components/ModalFooter.svelte"
  import {getEditorOptions, getEditorTags} from "@lib/editor"
  import {getPubkeyHints, publishComment} from "@app/commands"
  import {tagRoom, GENERAL} from "@app/state"
  import {pushToast} from "@app/toast"

  export let url
  export let event
  export let onClose
  export let onSubmit

  const submit = () => {
    if ($loading) return

    const content = $editor.getText({blockSeparator: "\n"})
    const tags = append(tagRoom(GENERAL, url), getEditorTags($editor))

    if (!content.trim()) {
      return pushToast({
        theme: "error",
        message: "Please provide a message for your reply.",
      })
    }

    onSubmit(publishComment({event, content, tags, relays: [url]}))
  }

  let editor: Readable<Editor>

  $: loading = $editor?.storage.fileUpload.loading

  onMount(() => {
    editor = createEditor(getEditorOptions({submit, getPubkeyHints, autofocus: !isMobile}))
  })
</script>

<form
  in:fly
  out:slideAndFade
  on:submit|preventDefault={submit}
  class="card2 sticky bottom-2 z-feature mx-2 mt-4 bg-neutral">
  <div class="relative">
    <div class="note-editor flex-grow overflow-hidden">
      <EditorContent editor={$editor} />
    </div>
    <Button
      data-tip="Add an image"
      class="tooltip tooltip-left absolute bottom-1 right-2"
      on:click={$editor.commands.selectFiles}>
      {#if $loading}
        <span class="loading loading-spinner loading-xs"></span>
      {:else}
        <Icon icon="paperclip" size={3} />
      {/if}
    </Button>
  </div>
  <ModalFooter>
    <Button class="btn btn-link" on:click={onClose}>Cancel</Button>
    <Button type="submit" class="btn btn-primary">Post Reply</Button>
  </ModalFooter>
</form>
