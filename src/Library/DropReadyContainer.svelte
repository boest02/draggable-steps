<!--
    This component will allow you to wrap a 'target' element as able
    to recieve a dropped element onto it.  When element is dropped
    the transmitted object will hydrated and passed to the dropHandler function
    Props:
      - dropHandler is a function pointer that can take the source and target objects
	  - targetObject is the object that contains info about the target element  
-->
<script>
    export let dropHandler;
	export let targetObject;
    
    function dragover (ev) {
		ev.preventDefault();
		ev.target.style.cursor = "grab";
	}
	
	const convertStringifiedObject = (jsonString) => {
		return JSON.parse(jsonString);
	};
	
    const drop = (ev) => {
		console.log("drop");
		ev.preventDefault();
        
        let sourceObject = convertStringifiedObject(ev.dataTransfer.getData("sourceObject"));
		console.dir(sourceObject);
		dropHandler(sourceObject, targetObject);
	}
</script>

<div on:drop={event => drop(event)} 
	 on:dragover={event => dragover(event)}>
    <slot><!-- ready for components --></slot>
</div>


