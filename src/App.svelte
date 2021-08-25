<script>
	import { writable } from "svelte/store";

	export const sharepic = writable({
		type: "WIDERLEGT",
		claim: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.",
		fact: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.",
		source: "correctiv.org",
		date: "01.01.2021",
	});

	const save = () => {
		const canvas = document.getElementsByTagName("canvas")[0].toDataURL();

		let link = document.createElement("a");
		if (typeof link.download === "string") {
			link.href = canvas;
			link.download = "fact.png";

			document.body.appendChild(link);

			link.click();

			document.body.removeChild(link);
		} else {
			window.open(canvas);
		}
	};

	const render = () => {
		setTimeout(() => {
			let svgElements = document.body.querySelectorAll("svg");
			svgElements.forEach(function (item) {
				item.setAttribute("width", item.getBoundingClientRect().width);
				item.setAttribute(
					"height",
					item.getBoundingClientRect().height
				);
				item.style.width = null;
				item.style.height = null;
			});

			html2canvas(document.querySelector("#capture"), {
				scale: 2,
				allowTaint: true,
				backgroundColor: "rgba(0,0,0,0)",
			}).then((canvas) => {
				const previous = document.getElementById("sharepicres");
				if (previous) document.getElementById("sharepicres").remove();
				canvas.setAttribute("id", "sharepicres");
				const main = document.getElementsByTagName("main")[0];
				main.appendChild(canvas);
			});
		}, 100);
	};

	const inithtml2canvas = () => {
		render();
	};
</script>

<svelte:head>
	<script
		src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"
		on:load={inithtml2canvas}></script>

	<title>Sharepic</title>
</svelte:head>

<main>
	<div
		class="sharepic"
		class:color-wrong={$sharepic.type == "WIDERLEGT"}
		class:color-right={$sharepic.type == "BESTÄTIGT"}
		class:color-partly-wrong={$sharepic.type == "IRREFÜHREND"}
	>
		<div class="header">
			<span class="type-edit">
				<select
					id="type"
					name="type"
					bind:value={$sharepic.type}
					on:input={() => render()}
				>
					<option value="WIDERLEGT">WIDERLEGT</option>
					<option value="BESTÄTIGT">BESTÄTIGT</option>
					<option value="IRREFÜHREND">IRREFÜHREND</option>
				</select>
			</span>
			<div class="fff">
				<img height="45px" src="./logo.png" alt="Facts For Friends" />
			</div>
		</div>
		<div class="body">
			<div class="wrapper">
				<div class="claim">
					<div class="title">BEHAUPTUNG</div>
					<textarea
						rows="3"
						bind:value={$sharepic.claim}
						on:input={() => render()}
					/>
				</div>
				<!-- <hr
					class:color-wrong={$sharepic.type == "WIDERLEGT"}
					class:color-right={$sharepic.type == "BESTÄTIGT"}
					class:color-partly-wrong={$sharepic.type == "IRREFÜHREND"}
				/> -->
				<div class="fact">
					<div class="title">FAKT</div>
					<textarea
						rows="7"
						bind:value={$sharepic.fact}
						on:input={() => render()}
					/>
				</div>
			</div>
			<div class="footer">
				<div class="source">
					Quelle: <input
						type="text"
						bind:value={$sharepic.source}
						on:input={() => render()}
					/>
				</div>
				<div class="date">
					Datum: <input
						type="text"
						bind:value={$sharepic.date}
						on:input={() => render()}
					/>
				</div>
			</div>
			<div class="logo">factsforfriends.de</div>
		</div>
	</div>

	<div
		id="capture"
		class="sharepic hide"
		class:color-wrong={$sharepic.type == "WIDERLEGT"}
		class:color-right={$sharepic.type == "BESTÄTIGT"}
		class:color-partly-wrong={$sharepic.type == "IRREFÜHREND"}
	>
		<div class="header">
			<span
				class="type"
				class:color-wrong={$sharepic.type == "WIDERLEGT"}
				class:color-right={$sharepic.type == "BESTÄTIGT"}
				class:color-partly-wrong={$sharepic.type == "IRREFÜHREND"}
				>{$sharepic.type}</span
			>
			<div class="fff">
				<img height="45px" src="./logo.png" alt="Facts For Friends" />
			</div>
		</div>
		<div class="body">
			<div class="wrapper">
				<div class="claim">
					<div class="title">BEHAUPTUNG</div>
					<div class="text">{$sharepic.claim}</div>
				</div>
				<!-- <hr
					class:color-wrong={$sharepic.type == "WIDERLEGT"}
					class:color-right={$sharepic.type == "BESTÄTIGT"}
					class:color-partly-wrong={$sharepic.type == "IRREFÜHREND"}
				/> -->
				<div class="fact">
					<div class="title">FAKT</div>
					<div class="text">{$sharepic.fact}</div>
				</div>
			</div>
			<div class="footer">
				<div class="source">
					Quelle: {$sharepic.source}
				</div>
				<div class="date">
					Datum: {$sharepic.date}
				</div>
			</div>
			<div class="logo">factsforfriends.de</div>
		</div>
	</div>

	<button on:click={save}> Download </button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	.color-right {
		background-color: #a3d8be;
		border: 4px solid #a3d8be;
	}

	.color-wrong {
		background-color: #fc6d72;
		border: 4px solid #fc6d72;
	}

	.color-partly-wrong {
		background-color: #c7c7c7;
		border: 4px solid #c7c7c7;
	}

	.wrapper {
		background-color: white;
		margin: -20px;
		padding: 20px;
		margin-top: -10px;
	}
	.fff {
		height: 100%;
		position: absolute;
		top: 2px;
	}

	.fff div {
		display: flex;
		justify-content: center;
		align-items: center;
		position: relative;
	}

	.fff .fff-title {
		margin: auto;
		margin-right: 0;
		margin-top: 5px;
		font-size: 1.1em;
	}

	.fff div svg {
		height: 35px;
		width: 35px;
	}

	.logo {
		color: #0b6b64;
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		margin: 5px auto;
	}

	.sharepic {
		margin: 20px auto;
		height: 500px;
		width: 500px;
		color: #ffffff;
		border-radius: 3px;
		z-index: -1;
		display: flex;
		flex-direction: column;
	}

	.hide {
		position: absolute;
		top: -5000px;
	}

	.sharepic .header .type {
		font-size: 1.2em;
		position: absolute;
		height: 100%;
		width: 180px;
		top: 0;
		left: 0;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.type-edit {
		position: absolute;
		top: 0;
		left: 0;
	}

	.sharepic .header {
		display: flex;
		justify-content: flex-end;
		height: 40px;
		position: relative;
	}

	.sharepic .body {
		flex: 1;
		justify-content: space-between;
		margin: 0 5px 5px 5px;
		padding: 0px 25px 25px 25px;
		position: relative;
		display: flex;
		justify-content: space-around;
		flex-direction: column;
	}
	.sharepic .body .claim,
	.sharepic .body .fact {
		display: flex;
		flex-direction: column;
		color: #4f4f4f;
		text-align: start;
	}

	.sharepic .body .claim .text,
	.sharepic .body .fact .text {
		border: none;
		padding: 10px;
	}
	.sharepic .body .claim {
		margin-bottom: 15px;
	}

	.sharepic .body textarea {
		resize: none;
	}

	.sharepic .body .title {
		font-size: 1.4em;
		font-weight: 600;
		margin-left: 10px;
	}

	.sharepic .body .text {
		border: none;
		padding: 10px;
	}

	.sharepic .body hr {
		width: 100%;
		height: 4px;
		border: none;
		margin-bottom: 0;
	}

	.sharepic input {
		margin: 0;
	}

	.sharepic .body .footer {
		display: flex;
		justify-content: space-between;
	}

	.sharepic .body .footer div {
		display: flex;
		align-items: center;
		color: white;
	}

	.sharepic .body .footer .source input {
		width: auto;
	}

	.sharepic .body .footer .date input {
		width: 130px;
	}
</style>
