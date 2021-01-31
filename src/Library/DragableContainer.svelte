<script>
    export let position;
    export let type;
    export let origin
    
    const dragstart =  (ev, offset) => {
		console.log("drag start " + offset);
		ev.dataTransfer.setData("offset", offset);
        ev.dataTransfer.setData("type", type);
        ev.dataTransfer.setData("origin", origin);
	};
</script>

<div draggable="true"
     on:dragstart={event => dragstart(event, position)}>
    <slot><!-- ready for components --></slot>
</div>


