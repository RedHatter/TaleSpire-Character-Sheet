<script lang="ts">
  import ConfigDialog from '../components/ConfigDialog.svelte'
  import Select from '../components/Select.svelte'
  import TextField from '../components/TextField.svelte'
  import Paper from '../components/Paper.svelte'
  import Cog from '../components/Cog.svelte'
  import { OtherProficiency, OtherProficiencyType } from './model'
  import type { DnD5eData } from './model'
  import { enumToSelect } from '../utils'

  export let data: DnD5eData

  let showDialog = false

  function close() {
    data = data
    showDialog = false
  }

  const items = enumToSelect(OtherProficiencyType)
</script>

{#if showDialog}
  <ConfigDialog on:close={close} bind:items={data.proficiencies} type={OtherProficiency} let:index>
    <div class="gap-2 flex mb-4">
      <Select label="Type" {items} bind:value={data.proficiencies[index].type} />
      <TextField label="Proficiency" placeholder="Common, Elvish" bind:value={data.proficiencies[index].name} />
      <TextField label="Source" placeholder="Race: Elf" bind:value={data.proficiencies[index].source} />
    </div>
  </ConfigDialog>
{/if}

<Paper class="relative w-full" title="Other Proficiencies">
  <Cog bind:value={showDialog} />
  <div class="table">
    <span class="header">Type</span><span class="header">Proficiency</span>
    {#each data.proficiencies as proficiency}
      <span>{OtherProficiencyType[proficiency.type]}</span>
      <span>{proficiency.name}</span>
    {/each}
  </div>
</Paper>

<style>
  .table {
    grid-template-columns: auto auto;
  }
</style>
