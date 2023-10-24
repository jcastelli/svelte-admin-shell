<script>
  import AdminShell from '$lib/AdminShell.svelte'
  import Header1 from './Header1.svelte'
  import Header2 from './Header2.svelte'
  import Header3 from './Header3.svelte'

  const constructors = [
    {
      constructor: Header1,
      title: "Main Corporate header",
      helpText: "If you don't know what to pick, choose this one..."
    },
    {
      constructor: Header2,
      name: "Header2",
      title: "A the second header",
      helpText: "This is just a static component."
    },
    {
      constructor: Header3,
      name: "ProvidedMachineName",
      title: "A regular old test component",
      helpText: "This is just a static component."
    },
  ]

  let editable = true
</script>


<p>Toggle editable mode: <button on:click={()=> editable = !editable}>editable = {editable}</button></p>
<AdminShell {constructors} {editable} componentGroupTitle="header" />


<style lang="scss">

</style>