<script lang="ts">
  import Button from "@lib/components/Button.svelte"
  import Link from "@lib/components/Link.svelte"
  import Icon from "@lib/components/Icon.svelte"
  import ModalHeader from "@lib/components/ModalHeader.svelte"
  import ModalFooter from "@lib/components/ModalFooter.svelte"
  import SpaceInviteAccept from "@app/components/SpaceInviteAccept.svelte"
  import {PLATFORM_NAME} from "@app/state"
  import {pushModal} from "@app/modal"

  const back = () => history.back()

  const next = () => {
    window.open("https://relay.tools/signup")

    setTimeout(() => pushModal(SpaceInviteAccept), 300)
  }
</script>

<form class="column gap-4" on:submit|preventDefault={next}>
  <ModalHeader>
    <div slot="title">Create a Space</div>
    <div slot="info">Host your own space, for your community.</div>
  </ModalHeader>
  <p>
    <Link class="text-primary" external href="https://relay.tools">relay.tools</Link> is a third-party
    service that allows anyone to run their own relay for use with {PLATFORM_NAME}, or any other
    nostr-compatible app.
  </p>
  <p>
    Once you've created a relay of your own, come back here to link {PLATFORM_NAME} with your new relay.
  </p>
  <ModalFooter>
    <Button class="btn btn-link" on:click={back}>
      <Icon icon="alt-arrow-left" />
      Go back
    </Button>
    <Button type="submit" class="btn btn-primary">
      Let's go
      <Icon icon="alt-arrow-right" />
    </Button>
  </ModalFooter>
</form>
