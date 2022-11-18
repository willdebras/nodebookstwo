<script>
	import { fade } from 'svelte/transition';
	import { storeFE } from '../store.js';
	export let objAttributes = {};
	import AceEditor from "../components/AceEditor.svelte"
	import "brace/mode/json"
  	import "brace/theme/chrome"

	let editorValue = ''

	// arrays to send globally to access the connection values
	let nodeTops = []
	let nodeBottoms = []

	// global bound variable containing selection of nodes
    const update = () => {
		selectionOfCheckboxes = selectionOfCheckboxes.concat(nodeTops)
		//selectionOfCheckboxes = startNode
    }

    // when group changes, update
    $: nodeTops, update()

	export let selectionOfCheckboxes
	

	// deleting a node
	function removeComponent() {
		$storeFE = $storeFE.filter(function(value, index, arr){ 
			if (value.id != objAttributes.id) return value
		})
		console.log($storeFE)
	}
    
	// variable flipped true when mouse down on header element of editor 
	let moving = false
	
	// only move nodes around if mouse is down on the header
	function onMouseDown() {
		moving = true;
	}
	
	// when mouse down on header, update the transform values
	function onMouseMove(e) {

		// handles movement on nodes
		if (moving) {
			document.activeElement.blur();
			objAttributes.left += e.movementX;
			objAttributes.top += e.movementY;
		}

	}

	// when enter key is pressed when in the node, unfocus it ending the editing
	function onEnterKey(e) {
		if(e.key === 'Enter') {
			document.activeElement.blur();
		}
	}
	
	function onMouseUp() {
		moving = false;
	}

	let startNode, elmPoint, endNode
	let connectMode, line

</script>

<div class='draggable' style="left: {objAttributes.left}px; top: {objAttributes.top}px;">
	<label for="checkbox_top_{objAttributes.id}" class='checkcontainer topNodeCheck' >
		<input id="checkbox_top_{objAttributes.id}" bind:group={nodeTops} type="checkbox" value="checkbox_top_{objAttributes.id}">
		<span class='checkmark topNodeSpan' bind:this={endNode}></span>
	</label>
    <div class='header' on:mousedown={onMouseDown}>
        <h1 class='nodeheader' contenteditable="true" on:keydown={onEnterKey}>New Node {objAttributes.id}</h1>
        <button class='removeButton' on:click={removeComponent}>x</button>
    </div>
    <div class='body'>
		<AceEditor
		on:selectionChange={(obj) => console.log(obj.detail)}
		on:paste={(obj) => console.log(obj.detail)}
		on:input={(obj) => console.log(obj.detail)}
		on:focus={() => console.log('focus')}
		on:documentChange={(obj) => console.log(`document change : ${obj.detail}`)}
		on:cut={() => console.log('cut')}
		on:cursorChange={() => console.log('cursor change')}
		on:copy={() => console.log('copy')}
		on:init={(editor) => console.log(editor.detail)}
		on:commandKey={(obj) => console.log(obj.detail)}
		on:changeMode={(obj) => console.log(`change mode : ${obj.detail}`)}
		on:blur={() => console.log('blur')}
		width='100%'
		height='100%'
		lang="json"
		theme="chrome"
		value={AceEditor} />
    </div>
	<label for="checkbox_bottom_{objAttributes.id}" class='checkcontainer bottomNodeCheck' >
		<input id="checkbox_bottom_{objAttributes.id}" bind:group={nodeBottoms} type="checkbox" value="checkbox_bottom_{objAttributes.id}">
		<span class='checkmark bottomNodeSpan' bind:this={startNode}></span>
	</label>
</div>

<svelte:window on:mouseup={onMouseUp} on:mousemove={onMouseMove} />

<style>

    .header{
        padding:4px 10px;
        background:lightgrey;
        margin-bottom:8px;
        border-bottom:1px solid gray;
        display:flex;
        justify-content: space-between;
        align-items: baseline;
    }

    .removeButton {
        padding: 0 6px 4px 6px;
        border-radius:4px;
        margin:0;
    }

	.removeButton:hover {
		cursor:pointer;
		border:1px solid orange;
	}

    h1 {
        font-size:16px;
        font-weight:400;
        margin:0;
    }

	*[contenteditable="true"]{
		display: inline-block;
	}

	.draggable {
		border: solid 1px gray;
		position: absolute;
        padding: 0; 
        width: 275px;
        height: 350px;
		resize:both;
		overflow: auto;
		/* overflow: visible; */
		/* overflow:hidden; */
	}

	.body {
		user-select: none;
		cursor: move;
		height:200px;
		height: calc(100% - 60px);
	}

	:global(.ace_scrollbar-h) {
		width: calc(100% - 50px) !important;
	}

	:global(.ace_scrollbar-v) {
		height: calc(100% - 20px) !important;
	}

	input[type='checkbox'] {
		transform: scale(0);
		height:0;
		width:0;
		margin:0;
		display:none;
	}

	/* Create a custom checkbox */
	.checkmark {
	position: absolute;
	top: 0;
	left: 0;
	height: 10px;
	width: 16px;
	background-color: lightyellow;
	cursor:pointer;
	}

	.checkmark:hover {
		background-color:orange;
	}

	.bottomNodeCheck {
		top: calc(100% - 11px);
		left: calc(50% - 6px);
		position:absolute;
	}

	.bottomNodeCheck > span {
		border: 1px solid gray;
		border-bottom: none;
	}

	.topNodeCheck {
		top:0;
		left: calc(50% - 6px);
		position:absolute;
	}

	.topNodeCheck > span {
		top:0;
		border: 1px solid white;
		border-top:none;
	}

/* When the checkbox is checked, add a blue background */
.checkcontainer input:checked ~ .checkmark {
  background-color: #2196F3;
}


</style>