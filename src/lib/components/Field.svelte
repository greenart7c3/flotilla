<div class="flex flex-col gap-2 {$$props.class}">
  {#if $$slots.label}
    <label class="flex items-center gap-2 font-bold">
      <slot name="label" />
    </label>
  {/if}
  <slot name="input" />
  {#if $$slots.info}
    <p class="text-sm">
      <slot name="info" />
    </p>
  {/if}
</div>
