<!-- 
   This component will allow you to wrap a 'source' element as dragabble...
   ...it will take an object, stringify it and when dropped onto onto
   a DropReadyContainer, it can be passed for processing as needed...
   Props:
      - sourceObject to be transmitted
-->
<script>
import App from "../App.svelte";

    export let sourceObject;
    
    const stringifyObject = (object) => {
        return JSON.stringify(object);
    }
    
    const dragstart =  (ev, sourceObject) => {
        ev.dataTransfer.setData("sourceObject", stringifyObject(sourceObject));
	};
</script>

<div draggable="true"
     on:dragstart={event => dragstart(event, sourceObject)}>
    <slot><!-- ready for components --></slot>
</div>


