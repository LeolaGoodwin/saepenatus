<script lang="ts">
  import { _ } from 'svelte-i18n'
  import en from '../i18n/en.json'
  import { getDevice } from '../utils'

  import SimulationHeader from './components/SimulationHeader.svelte'
  import IconBadge from './components/IconBadge.svelte'
  import closeIcon from '../icons/close-circle.js'
  import Button from './components/Button.svelte'

  export let toggleExpanded: (maximize: boolean) => void
  export let destroyApp: () => void
  export let startTime: number

  const device = getDevice()
</script>

<style>
  .minimized {
    transition: background 300ms ease-in-out, color 300ms ease-in-out;
    pointer-events: all;
    backdrop-filter: blur(5px);
    width: 100%;
    min-height: 3.5rem;
    background: var(--foreground-color);
    display: flex;
    flex-direction: column;
    position: relative;
    overflow: hidden;
  }

  .radius {
    border-radius: inherit;
  }

  .tp-header {
    padding: 0.75rem;
    border-bottom: var(--border-color, transparent) 1px solid;
  }

  div.tp-close-btn {
    visibility: visible;
    opacity: 1;
  }

  div.tp-close-btn {
    margin-left: auto;
    margin-bottom: auto;
    height: 1.5rem;
    width: 1.5rem;
    position: absolute;
    top: 0.5rem;
    right: 0.5rem;
    justify-content: center;
    align-items: center;
  }

  div.minimized:hover > div.tp-close-btn-desktop {
    visibility: visible;
    opacity: 1;
  }

  div.tp-close-btn-desktop {
    visibility: hidden;
    transition: visibility 0.15s linear, opacity 0.15s linear;
    opacity: 0;
  }

  .tp-close-btn .close-icon {
    width: 1.25rem;
    margin: auto;
  }

  .tp-close-btn > .close-icon {
    color: var(--tp-close-icon-color, var(--onboard-gray-300, var(--gray-300)));
  }

  .tp-close-btn:hover > .close-icon {
    color: var(--tp-close-icon-hover, var(--onboard-gray-100, var(--gray-100)));
  }

  .details-cta {
    font-weight: 700;
    font-size: 0.875rem;
    display: flex;
    justify-content: flex-end;
    flex-direction: row;
    align-items: center;
    padding: 0.5rem;
    gap: 0.5rem;
    height: 3rem;
    border-top: 1px solid var(--border-color);
    flex: none;
    order: 2;
    align-self: stretch;
    flex-grow: 0;
  }
</style>

<div class="minimized radius padding-5">
  <div
    on:click|stopPropagation={() => {
      destroyApp()
    }}
    class="tp-close-btn tp-close-btn-{device.type} pointer flex"
  >
    <div class="flex items-center close-icon">
      {@html closeIcon}
    </div>
  </div>
  <div class="flex tp-header">
    <IconBadge />
    <SimulationHeader {startTime} />
  </div>
  <div class="details-cta">
    <Button
      btnText={$_('minimized.show', {
        default: en.minimized.show
      })}
      btnFunction={() => toggleExpanded(true)}
    />
  </div>
</div>
