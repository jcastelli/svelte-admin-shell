<script>
  import debugString from '$lib/imports/debugString'
  export let data = undefined
   
  // $: console.log('onscreen data:', data)
</script>

<pre class="debug">
  { data ? debugString(actualData): 'No data provided' }
</pre>