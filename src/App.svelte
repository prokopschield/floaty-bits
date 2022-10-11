<script lang="ts">
	let number = 0xdeadbeef;

	$: float64array = new Float64Array([number]);
	$: float32array = new Float32Array([number]);

	$: float64buffer = float64array.buffer;
	$: float32buffer = float32array.buffer;

	$: int64array = new BigUint64Array(float64buffer);
	$: int32array = new Uint32Array(float32buffer);

	$: bits64 = int64array[0].toString(2);
	$: bits32 = int32array[0].toString(2);

	$: bits64full = ('0'.repeat(64) + bits64).slice(-64);
	$: bits32full = ('0'.repeat(32) + bits32).slice(-32);

	$: bits64ar = bits64full.split('').map((a) => a == '1');
	$: bits32ar = bits32full.split('').map((a) => a == '1');

	$: hex_number = '0x' + number.toString(16);
	$: oct_number = '0o' + number.toString(8);
	$: bin_number = '0b' + number.toString(2);

	let calc = '';
</script>

<main>
	<div class="center">
		<table>
			<tr>
				<td>
					<h3>Calculator</h3>
				</td>
				<td>
					<input
						type="text"
						bind:value={calc}
						on:keyup={() => {
							let result = Number(eval(calc));

							if (result === result) {
								number = result;
							}
						}}
					/>
				</td>
			</tr>
			<tr>
				<td>
					<h3>Decimal</h3>
				</td>
				<td>
					<input type="number" bind:value={number} />
				</td>
			</tr>
			<tr>
				<td>
					<h3>Hex</h3>
				</td>
				<td>
					<input
						type="text"
						bind:value={hex_number}
						on:keyup={() => {
							const n = Number(hex_number);
							if (n === n) {
								number = n;
							}
						}}
					/>
				</td>
			</tr>
			<tr>
				<td>
					<h3>Octal</h3>
				</td>
				<td>
					<input
						type="text"
						bind:value={oct_number}
						on:keyup={() => {
							const n = Number(oct_number);
							if (n === n) {
								number = n;
							}
						}}
					/>
				</td>
			</tr>
			<tr>
				<td>
					<h3>Binary</h3>
				</td>
				<td>
					<input
						type="text"
						bind:value={bin_number}
						on:keyup={() => {
							const n = Number(bin_number);
							if (n === n) {
								number = n;
							}
						}}
					/>
				</td>
			</tr>
		</table>
	</div>
	<div class="center">
		<table>
			<tr>
				{#each bits64ar as b, i}
					<td>
						<span>{i}</span>
						<br />
						<span>{b ? 1 : 0}</span>
						<br />
						<input
							type="checkbox"
							bind:checked={bits64ar[i]}
							on:click={() => {
								setTimeout(() => {
									int64array[0] = BigInt(`0b${bits64ar.map(Number).join('')}`);
									number = float64array[0];
								});
							}}
						/>
					</td>
				{/each}
			</tr>
			<tr>
				{#each bits32ar as b, i}
					<td>
						<span>{i}</span>
						<br />
						<span>{b ? 1 : 0}</span>
						<br />
						<input
							type="checkbox"
							bind:checked={bits32ar[i]}
							on:click={() => {
								setTimeout(() => {
									int32array[0] = Number(`0b${bits32ar.map(Number).join('')}`);
									number = float32array[0];
								});
							}}
						/>
					</td>
				{/each}
			</tr>
		</table>
	</div>
</main>

<style>
	.center {
		display: flex;
		justify-content: center;
		align-items: center;
	}
	table {
		margin: 0.8em;
	}
	h3 {
		margin: 0.3em;
	}
	input {
		font-size: 1.4em;
	}
</style>
