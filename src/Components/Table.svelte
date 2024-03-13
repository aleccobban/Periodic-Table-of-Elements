<script>
  import { elements } from '../Data/PeriodicTableJSON.json';
  import Element from './Element.svelte';
  import Modal from './Modal.svelte';
  let showModal = false
  let modalAtomicNumber=null

  function handleMessage(event){
    console.log(event);
    triggerShowModal(event.detail.atomicNumber)
  }
  function triggerShowModal(atomicNumber){
    console.log(elements[atomicNumber-1])
    
    if(elements[atomicNumber-1]?.bonds_n){
      showModal=true
      modalAtomicNumber=atomicNumber
    }
    else{
      console.log(`inside the else statement`)
      showModal=false
      modalAtomicNumber=null
    }
  }

</script>

<style>
  .table {
    display: grid;
    grid-template-columns: repeat(18, auto) 1fr;
    gap: 0.1vw;
  }

</style>



<div>
  <div class="table text-gray-900">
    {#each elements as element, i} 
    

      <Element on:details={handleMessage}
        atomicNumber={element.number}
        style="grid-column: {element.xpos}; grid-row: {element.ypos}" >
  </Element>
    {:else}{null}{/each}
  </div>
</div>

{#if showModal&&modalAtomicNumber}
<Modal bind:showModal>
	<h2 slot="header">
		{elements[modalAtomicNumber-1]?.name??''}
		<small><em>{elements[modalAtomicNumber-1]?.symbol??''}</em></small>
    <h3>Examples</h3>
    <ol>
      {#each elements[modalAtomicNumber-1]?.bonds??[] as bond}
      <li>
      <div>Name: {bond.name}</div>
      </li>  
      <li>
      <div>Structure: {bond.structure}</div>
      </li>  
      <li>
      <img src={bond.bond_image}/>
        </li> 
      {/each}
  </ol>
	</h2>
</Modal>
{:else}<div/>{/if}

