<script>
	import DragableContainer from './Library/DragableContainer.svelte';
	import components from './data.js';
	import samplesteps from './steps.js';
  import library from './library.json';
	import Icon from 'svelte-awesome';
  import { trash, pencil } from 'svelte-awesome/icons';
  	
	let stepsExpanded = true;
	let steps = samplesteps;
	let path_components = [];
	let step = {
		"components": []
	};
		
	function updateSession(stepNumber) {
		steps.splice(stepNumber, 1);
		steps = steps; //make reactive
	}
	function removeStep(stepNumber) {
		steps.splice(stepNumber, 1);
		steps = steps; //make reactive
	}
	function removeItem(stepNumber, itemNumber) {
		steps[stepNumber].components.splice(itemNumber, 1);
		steps = steps; //make reactive
	}
	function editItem(stepNumber, itemNumber) {	
		alert("Coming Soon!");
		steps = steps; //make reactive
	}
	function addStep() {
		console.log("add step");
		steps.push(JSON.parse(JSON.stringify(step)));
		steps = steps; //make reactive
	}
	
	function dragover (ev) {
		console.log("drag over");
		ev.preventDefault();
		ev.target.style.cursor = "grab";
	}
	function drop (ev, stepNumber, position) {
		console.log("drop");
		ev.preventDefault();
		let offset = ev.dataTransfer.getData("offset");
		let type = ev.dataTransfer.getData("type");
		let origin = ev.dataTransfer.getData("origin");
		if(type === 'move') {
			if(origin != stepNumber) {
				console.log("Illegal move from step " + origin + " to step " + stepNumber);
				return;
			}
			let element = steps[stepNumber].components[offset];
			steps[stepNumber].components.splice(offset, 1);
			steps[stepNumber].components.splice(position, 0, element);
		} 
		else {
			steps[stepNumber].components.splice(position, 0, components[offset]);
		}
		steps = steps;
	}
</script>

<p>Drag a Component to the Step you would like, in the order they should appear...</p>

<fieldset class="components">    
		<legend>Components (Drag from Here...)</legend>
		<div class='holder'>
			{#each components as item, i}
			<!-- <div class='component' draggable="true" on:dragstart={event => dragstart(event, i)}>{item.component}</div> -->
			<DragableContainer position={i} type='add' origin=0> <div class="component">{item.component}</div></DragableContainer>
			{/each}
		</div>
</fieldset>  

<fieldset class="steps">
	<legend>Configured Steps (Drop Component on Step...)</legend>
	{#if stepsExpanded}
		<button title="Collapse" on:click={() => stepsExpanded = false}> - </button>
		{#each steps as step, p}
		<fieldset>    
			<legend>{"Step " + (p+1)}</legend>
			<div class='step-container'>
				{#if step.components.length}
					<div class='component disabled'
							on:drop={event => drop(event, p, 0)} 
							on:dragover={event => dragover(event)}>- First Position Drop -
					</div>
				{/if}
				{#each step.components as item,i}
					<DragableContainer position={i} type='move' origin={p}>
					<div class='component' on:drop={event => drop(event, p, i)} 
							on:dragover={event => dragover(event)}>
						{item.component} 
						<button class="delete-component"
										on:click={ev => removeItem(p,i)}
										title="Remove Component...">
							<Icon class="icon" inverse=true data={trash}/>
						</button>
						<button class="edit-component"
										on:click={ev => editItem(p,i)}>
							<Icon class="icon" inverse=true data={pencil}/>
						</button>
					</div>
					</DragableContainer>
				{/each}			
				{#if step.components.length}
					<div class='component disabled' 
							on:drop={event => drop(event, p, step.components.length)} 
							on:dragover={event => dragover(event)}>- Last Position Drop -
				</div>
				{:else}
					<div class='component empty'
							on:drop={event => drop(event, p, step.components.length)} 
							on:dragover={event => dragover(event)}>Add Components <br/> Here
					</div>
				{/if}

			</div>
			<section class="step-controls">
				<button on:click={() => removeStep(p)}>Remove Step</button>			
			</section>
		</fieldset>
		{/each}
		<fieldset class="more-button">			
			<button title="Add a Step..." on:click={addStep}> Add a Step </button>	
	</fieldset>
{:else}
	<button title="Expand" on:click={() => stepsExpanded = true}> + </button>
{/if}
</fieldset>

<fieldset class="output">
	<legend>Output (Edit as Needed...)</legend>
	<textarea>
		{JSON.stringify(steps, null, '\t')}
	</textarea>
</fieldset>

<style>
	.components .holder {
		display: flex;		
		flex-wrap: wrap;
		padding: 0;
		margin: 0;
	}
	.components .component:hover {
		background-color: rgb(255,66,70);
		color: white;		
		cursor: grab;
	}
	.component:drop {
		cursor: grab;
	}	
	.components legend,
	.steps legend,
	.output legend {
		background: white;
		padding: 5px;
		font-weight: 600;
	}
	.steps fieldset {
		display: inline-block;
		vertical-align: top;
		border-radius: 4px;
		box-shadow: 2px 2px 4px 0px #888888;
	}
	.steps button {
		float: right;
		top: 0;
		font-weight: bold;
		padding: 0px 4px;
	}
	.more-button {
		margin-top: 13px;
		width: 237px;
	}
	.more-button button {
		width: 100%;
		margin-top: 15px;
		font-weight: 400;
	}
	.steps fieldset ul {
		display: flex;
		flex-direction: column;		
		padding: 0px 20px
	}
	section {
		display: flex;		
		font-size: 12px;
		font-weight: 600;
		justify-content: space-evenly
	}
	.session-fields div {
		padding: 5px;
	}
	section button {
		margin-top: 5px;
		padding: 3px 12px;
		box-shadow: 2px 2px 4px 0px #888888;
	}	
	.component {
		position: relative;
		padding: 10px;
		list-style-type: none;		
		background: #f9f9f9;		
		border: 1px solid black;
		border-radius: 5px;
		width: 170px;
		text-align: left;	
		margin: 2px;
		box-shadow: 2px 2px 4px 0px #888888;
		text-transform: capitalize;
	}
	.component button {
		position: absolute;
		background: none;
		border: none;
	}
	.delete-component {
		margin-top: 5px;
		right: 2px;
	}
	.edit-component {
		margin-top: 5px;
		right: 19px;
	}
	.steps fieldset .step-container .component {
		background-color: rgb(255,66,70);
		color: white;
		font-weight: bold;
		border: 1.5px solid black;
		box-shadow: 2px 2px 4px 0px #888888;
	}
	.steps fieldset .step-container .component.disabled {
		opacity: .4;
		color: black;
		background: white;
		padding: 2px 10px;
		text-align: center;
		font-size: 12px;
	}	
	.steps fieldset .step-container .component.empty {
		opacity: 1;
		color: darkgray;
		background: none;
		border: .5px solid darkgray;
		text-align: center;
		padding: 10px;
	}	
	.output {
		height: auto;
		width: 96%;
		padding-bottom: 15px;
	}
	textarea {
		display: block;
		height: 500px;
		width: 100%;
		box-shadow: 2px 2px 4px 0px #888888;
	}
</style>


