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
		class:color-partly-wrong={$sharepic.type == "TEILWEISE_WIDERLEGT"}
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
				<div>
					<div class="fff-title">Facts for Friends</div>
					<svg
						class="h-8 w-8 stroke-current fill-current inline text-highlight"
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 179 245"
					>
						<g data-name="Layer 2">
							<g data-name="Layer 1">
								<path
									stroke="#0b6b64"
									fill="none"
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="8.2"
									d="M82 130c1-5 4-24-4-30-4-3-13-3-16 2-7 8-3 25 4 35 8 12 16 13 22 13s16 1 25-11c8-12 10-32 2-39-5-4-13-3-16 1l-1 1c-6 9 0 28 0 28 3 11 6 14 5 17s-13 4-15 3c-3 0-4 0-7-2s-1-9 1-18z"
								/>
								<circle
									stroke="#0b6b64"
									cx="110.7"
									cy="73.8"
									r="11.2"
									stroke-width="8.2"
									fill="none"
								/>
								<circle
									stroke="#0b6b64"
									cx="68.5"
									cy="73.8"
									r="11.2"
									stroke-width="8.2"
									fill="none"
								/>
								<path
									fill="#0b6b64"
									d="M79 241a2 2 0 011-3h22c3 1 1 3 1 4 0 2-3 3-8 3h-8c-5 0-8-1-8-3z"
								/>
								<path
									stroke="#0b6b64"
									fill="none"
									stroke-miterlimit="10"
									stroke-width="8.2"
									d="M175 80s0 0 0 0c0-33-37-76-85-76h0C42 4 4 47 4 80c0 0 0 0 0 0 0 22 15 46 20 54l1 2c10 18 24 36 27 52l4 24h0l2 8c2 5 9 8 10 8 4 2 8 2 12 2h20a27 27 0 005 0 21 21 0 009-2c1-1 5-4 7-8l2-8s0 0 0 0l4-24c3-16 17-34 27-52l1-2c5-8 20-32 20-54z"
								/>
							</g>
						</g>
					</svg>
				</div>
			</div>
		</div>
		<div class="body">
			<div class="claim">
				<div class="title">BEHAUPTUNG</div>
				<textarea
					rows="3"
					bind:value={$sharepic.claim}
					on:input={() => render()}
				/>
			</div>
			<hr
				class:color-wrong={$sharepic.type == "WIDERLEGT"}
				class:color-right={$sharepic.type == "BESTÄTIGT"}
				class:color-partly-wrong={$sharepic.type == "IRREFÜHREND"}
			/>
			<div class="fact">
				<div class="title">FAKT</div>
				<textarea
					rows="7"
					bind:value={$sharepic.fact}
					on:input={() => render()}
				/>
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
				<div>
					<div class="fff-title">Facts for Friends</div>
					<svg
						class="h-8 w-8 stroke-current fill-current inline text-highlight"
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 179 245"
					>
						<g data-name="Layer 2">
							<g data-name="Layer 1">
								<path
									stroke="#0b6b64"
									fill="none"
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="8.2"
									d="M82 130c1-5 4-24-4-30-4-3-13-3-16 2-7 8-3 25 4 35 8 12 16 13 22 13s16 1 25-11c8-12 10-32 2-39-5-4-13-3-16 1l-1 1c-6 9 0 28 0 28 3 11 6 14 5 17s-13 4-15 3c-3 0-4 0-7-2s-1-9 1-18z"
								/>
								<circle
									stroke="#0b6b64"
									cx="110.7"
									cy="73.8"
									r="11.2"
									stroke-width="8.2"
									fill="none"
								/>
								<circle
									stroke="#0b6b64"
									cx="68.5"
									cy="73.8"
									r="11.2"
									stroke-width="8.2"
									fill="none"
								/>
								<path
									fill="#0b6b64"
									d="M79 241a2 2 0 011-3h22c3 1 1 3 1 4 0 2-3 3-8 3h-8c-5 0-8-1-8-3z"
								/>
								<path
									stroke="#0b6b64"
									fill="none"
									stroke-miterlimit="10"
									stroke-width="8.2"
									d="M175 80s0 0 0 0c0-33-37-76-85-76h0C42 4 4 47 4 80c0 0 0 0 0 0 0 22 15 46 20 54l1 2c10 18 24 36 27 52l4 24h0l2 8c2 5 9 8 10 8 4 2 8 2 12 2h20a27 27 0 005 0 21 21 0 009-2c1-1 5-4 7-8l2-8s0 0 0 0l4-24c3-16 17-34 27-52l1-2c5-8 20-32 20-54z"
								/>
							</g>
						</g>
					</svg>
				</div>
			</div>
		</div>
		<div class="body">
			<div class="claim">
				<div class="title">BEHAUPTUNG</div>
				<div class="text">{$sharepic.claim}</div>
			</div>
			<hr
				class:color-wrong={$sharepic.type == "WIDERLEGT"}
				class:color-right={$sharepic.type == "BESTÄTIGT"}
				class:color-partly-wrong={$sharepic.type == "IRREFÜHREND"}
			/>
			<div class="fact">
				<div class="title">FAKT</div>
				<div class="text">{$sharepic.fact}</div>
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
		background-color: #0b6b64;
		border: 4px solid #0b6b64;
	}

	.color-wrong {
		background-color: #c30303;
		border: 4px solid #c30303;
	}

	.color-partly-wrong {
		background-color: #c8c8c8;
		border: 4px solid #c8c8c8;
	}

	.fff {
		top: 0;
		right: 0;
		height: 100%;
		width: 180px;
		color: #0b6b64;
		font-weight: 300;
		font-size: 1.05em;
	}

	.fff div {
		display: flex;
		justify-content: center;
		align-items: center;
		position: relative;
		margin: auto;
		top: 5px;
		right: 5px;
	}

	.fff .fff-title {
		margin: auto;
		margin-right: 0;
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
		background: #fffdf4;
		background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAMAAAAp4XiDAAAAUVBMVEWFhYWDg4N3d3dtbW17e3t1dXWBgYGHh4d5eXlzc3OLi4ubm5uVlZWPj4+NjY19fX2JiYl/f39ra2uRkZGZmZlpaWmXl5dvb29xcXGTk5NnZ2c8TV1mAAAAG3RSTlNAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEAvEOwtAAAFVklEQVR4XpWWB67c2BUFb3g557T/hRo9/WUMZHlgr4Bg8Z4qQgQJlHI4A8SzFVrapvmTF9O7dmYRFZ60YiBhJRCgh1FYhiLAmdvX0CzTOpNE77ME0Zty/nWWzchDtiqrmQDeuv3powQ5ta2eN0FY0InkqDD73lT9c9lEzwUNqgFHs9VQce3TVClFCQrSTfOiYkVJQBmpbq2L6iZavPnAPcoU0dSw0SUTqz/GtrGuXfbyyBniKykOWQWGqwwMA7QiYAxi+IlPdqo+hYHnUt5ZPfnsHJyNiDtnpJyayNBkF6cWoYGAMY92U2hXHF/C1M8uP/ZtYdiuj26UdAdQQSXQErwSOMzt/XWRWAz5GuSBIkwG1H3FabJ2OsUOUhGC6tK4EMtJO0ttC6IBD3kM0ve0tJwMdSfjZo+EEISaeTr9P3wYrGjXqyC1krcKdhMpxEnt5JetoulscpyzhXN5FRpuPHvbeQaKxFAEB6EN+cYN6xD7RYGpXpNndMmZgM5Dcs3YSNFDHUo2LGfZuukSWyUYirJAdYbF3MfqEKmjM+I2EfhA94iG3L7uKrR+GdWD73ydlIB+6hgref1QTlmgmbM3/LeX5GI1Ux1RWpgxpLuZ2+I+IjzZ8wqE4nilvQdkUdfhzI5QDWy+kw5Wgg2pGpeEVeCCA7b85BO3F9DzxB3cdqvBzWcmzbyMiqhzuYqtHRVG2y4x+KOlnyqla8AoWWpuBoYRxzXrfKuILl6SfiWCbjxoZJUaCBj1CjH7GIaDbc9kqBY3W/Rgjda1iqQcOJu2WW+76pZC9QG7M00dffe9hNnseupFL53r8F7YHSwJWUKP2q+k7RdsxyOB11n0xtOvnW4irMMFNV4H0uqwS5ExsmP9AxbDTc9JwgneAT5vTiUSm1E7BSflSt3bfa1tv8Di3R8n3Af7MNWzs49hmauE2wP+ttrq+AsWpFG2awvsuOqbipWHgtuvuaAE+A1Z/7gC9hesnr+7wqCwG8c5yAg3AL1fm8T9AZtp/bbJGwl1pNrE7RuOX7PeMRUERVaPpEs+yqeoSmuOlokqw49pgomjLeh7icHNlG19yjs6XXOMedYm5xH2YxpV2tc0Ro2jJfxC50ApuxGob7lMsxfTbeUv07TyYxpeLucEH1gNd4IKH2LAg5TdVhlCafZvpskfncCfx8pOhJzd76bJWeYFnFciwcYfubRc12Ip/ppIhA1/mSZ/RxjFDrJC5xifFjJpY2Xl5zXdguFqYyTR1zSp1Y9p+tktDYYSNflcxI0iyO4TPBdlRcpeqjK/piF5bklq77VSEaA+z8qmJTFzIWiitbnzR794USKBUaT0NTEsVjZqLaFVqJoPN9ODG70IPbfBHKK+/q/AWR0tJzYHRULOa4MP+W/HfGadZUbfw177G7j/OGbIs8TahLyynl4X4RinF793Oz+BU0saXtUHrVBFT/DnA3ctNPoGbs4hRIjTok8i+algT1lTHi4SxFvONKNrgQFAq2/gFnWMXgwffgYMJpiKYkmW3tTg3ZQ9Jq+f8XN+A5eeUKHWvJWJ2sgJ1Sop+wwhqFVijqWaJhwtD8MNlSBeWNNWTa5Z5kPZw5+LbVT99wqTdx29lMUH4OIG/D86ruKEauBjvH5xy6um/Sfj7ei6UUVk4AIl3MyD4MSSTOFgSwsH/QJWaQ5as7ZcmgBZkzjjU1UrQ74ci1gWBCSGHtuV1H2mhSnO3Wp/3fEV5a+4wz//6qy8JxjZsmxxy5+4w9CDNJY09T072iKG0EnOS0arEYgXqYnXcYHwjTtUNAcMelOd4xpkoqiTYICWFq0JSiPfPDQdnt+4/wuqcXY47QILbgAAAABJRU5ErkJggg==);
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

	.sharepic .body textarea {
		resize: none;
	}

	.sharepic .body .title {
		font-size: 1.2em;
		padding-bottom: 5px;
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
		color: #8e8e8e;
	}

	.sharepic .body .footer .source input {
		width: auto;
	}

	.sharepic .body .footer .date input {
		width: 130px;
	}
</style>
