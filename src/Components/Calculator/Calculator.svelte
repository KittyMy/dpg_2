<style>
	.calculator {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%,-50%);
		width: 300px;
		background-color: #2d2d2f;
		box-shadow: 2px 2px 300px #000;
		border-radius: 2%;
	}
	.calculator .header {
		display: flex;
		width: 100%;
		margin: 10px 0 0;
	}
	.calculator .header .circle {
		border-radius: 50%;
		width: 15px;
		height: 15px;
		margin-left: 10px
	}
	.calculator .header .circle:nth-child(1) {
		background-color: #ec6559;
	}
	.calculator .header .circle:nth-child(2) {
		background-color: #e0c04b;
	}
	.calculator .header .circle:nth-child(3) {
		background-color: #71be46;
	}
	.calculator input {
		width: 100%;
		background-color: inherit;
		border: none;
		padding: 20px 15px 5px 15px;
		margin: 0;
		color: #e6e6e6;
		outline: none;
		text-align: right;
		font-size: 50px;
		border-radius: 3%;
	}
	.calculator .operations {
		display: flex;
		justify-content: space-between;
		align-items: center;
		flex-direction: column;
	}
	.calculator .operations button {
		background-color: #606061;
		color: #e6e6e6;
		width: 70px;
		height: 60px;
		border-radius: 6%;
		font-size: 25px;
		padding: 0;
		margin: 2px 0;
		border: none;
	}
	.calculator .operations .topBtn button.main,
	.calculator .operations button.main {
		background-color: #f2a13b;
	}
	.calculator .operations button.zero {
		width: 145px;
	}
	.calculator .operations button:hover {
	  cursor: pointer;
	}
	.calculator .operations .topBtn button {
		background-color: #404042;
	}
	.calculator button {
		outline: none;
	}
</style>

<div class="calculator">
	<div class="header">
		<span class="circle"></span>
		<span class="circle"></span>
		<span class="circle"></span>
	</div>
	<input type="text" bind:value={console} readonly="true"/>
	<div class="operations">
		<div class="topBtn">
			<button on:click={()=>{setValue('AC');}}>
				AC
			</button>
			<button on:click={()=>{setOperation('posneg');}}>
				+/-
			</button>
			<button on:click={()=>{setValue('percentage');}}>
				%
			</button>
			<button class="main" on:click={()=>{setOperation('divide');}}>
				&divide;
			</button>
		</div>
		<div>
			<button on:click={()=>{setValue(7);}}>
				7
			</button>
			<button on:click={()=>{setValue(8);}}>
				8
			</button>
			<button on:click={()=>{setValue(9);}}>
				9
			</button>
			<button class="main" on:click={()=>{setOperation('multiply');}}>
				&times;
			</button>
		</div>
		<div>
			<button on:click={()=>{setValue(4);}}>
				4
			</button>
			<button on:click={()=>{setValue(5);}}>
				5
			</button>
			<button on:click={()=>{setValue(6);}}>
				6
			</button>
			<button class="main" on:click={()=>{setOperation('substract');}}>
				-
			</button>
		</div>
		<div>
			<button on:click={()=>{setValue(1);}}>
				1
			</button>
			<button on:click={()=>{setValue(2);}}>
				2
			</button>
			<button on:click={()=>{setValue(3);}}>
				3
			</button>
			<button class="main" on:click={()=>{setOperation('add');}}>
				+
			</button>
		</div>
		<div>
			<button class="zero" on:click={()=>{setValue(0);}}>
				0
			</button>
			<button on:click={()=>{setValue('.');}}>
				.
			</button>
			<button class="main" on:click={equal}>
				=
			</button>
		</div>
	</div>
</div>

<script>
	let total = 0;
	let console = 0;
	let state = null;
	let toggled = false;
	function resolveState(){
		switch(state){
			case "add":
				total += parseFloat(console);
				console = "0";
				break;
			case "substract":
				total -= parseFloat(console);
				console = "0";
				break;
			case "multiply":
				total *= parseFloat(console);
				console = "0";
				break;
			case "divide":
				total /= parseFloat(console);
				console = "0";
				break;
		  case "posneg":
				// toggled = !toggled
				// let minus = toggled ? '-' : '';
				// state = total * -1;
				total *= -1;
				console = total;
				break;
			default:
				total = parseFloat(console);
				console = "0";
				break;
		}
	}
	function setOperation(operation){
		resolveState();
		state = operation;
	}
	function setValue(value){
		if(console.toString() == "0" || state == "equal"){
			console = "";
		}
		if(state == "equal"){
			state = null;
		}
		if(value == "AC"){
			total = 0;
			state = null;
			console = "0";
			return;
		}
		if(value == "percentage"){
			state = parseFloat(console) / 100;
			value = "";
			console = state;
		}
		console = console + value;
	}
	function equal(){
		resolveState();
		console = total;
		state = "equal";
	}
</script>
