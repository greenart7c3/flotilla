<script lang="ts">
  import Icon from "@lib/components/Icon.svelte"
  import Button from "@lib/components/Button.svelte"
  import Divider from "@lib/components/Divider.svelte"
  import CardButton from "@lib/components/CardButton.svelte"
  import MenuSpacesItem from "@app/components/MenuSpacesItem.svelte"
  import SpaceAdd from "@app/components/SpaceAdd.svelte"
  import {userMembership, getMembershipUrls, PLATFORM_RELAY} from "@app/state"
  import {pushModal} from "@app/modal"

  const addSpace = () => pushModal(SpaceAdd)
</script>

<div class="column menu gap-2">
  {#if PLATFORM_RELAY}
    <MenuSpacesItem url={PLATFORM_RELAY} />
    <Divider />
  {:else if getMembershipUrls($userMembership).length > 0}
    {#each getMembershipUrls($userMembership) as url (url)}
      <MenuSpacesItem {url} />
    {/each}
    <Divider />
  {/if}
  {#if !PLATFORM_RELAY}
    <Button on:click={addSpace}>
      <CardButton>
        <div slot="icon"><Icon icon="login-2" size={7} /></div>
        <div slot="title">Add a space</div>
        <div slot="info">Join or create a new space</div>
      </CardButton>
    </Button>
  {/if}
</div>
