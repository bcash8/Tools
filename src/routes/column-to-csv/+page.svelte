<script lang="ts">
	let columnData = $state('');
	let csvData = $state('');
	let delimiter = $state(',');
	let prefix = $state('');
	let suffix = $state('');
	let lastEdited = $state('column');

	$effect(() => {
		if (lastEdited === 'column') {
			const convertedColumnData =
				columnData
					.trim()
					.split(/\r\n|\r|\n/)
					?.map((row) => `${prefix}${row}${suffix}`)
					.join(delimiter) ?? '';
			csvData = convertedColumnData;
		}
	});

	$effect(() => {
		if (lastEdited === 'csv') {
			const escapedPrefix = escapeRegex(prefix);
			const escapedSuffix = escapeRegex(suffix);

			const dataRegex = new RegExp(
				`(?<=${escapedPrefix})[^${delimiter}]+(?=${escapedSuffix})`,
				'g'
			);
			const data = [...csvData.matchAll(dataRegex)];
			columnData = data.join('\n');
		}
	});

	function escapeRegex(str: string): string {
		return str.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
	}
</script>

<svelte:head>
	<title>Column To CSV</title>
	<meta
		name="description"
		content="Convert column of data to a comma seperated list of data for free"
	/>
</svelte:head>

<main>
	<div class="setting-inputs">
		<input type="text" placeholder="delimiter" bind:value={delimiter} />
		<input type="text" placeholder="prefix" bind:value={prefix} />
		<input type="text" placeholder="suffix" bind:value={suffix} />
	</div>
	<div class="data-inputs">
		<textarea
			placeholder="Paste column data here"
			bind:value={columnData}
			oninput={() => (lastEdited = 'column')}
		></textarea>
		<textarea
			placeholder="Paste CSV data here"
			bind:value={csvData}
			oninput={() => {
				lastEdited = 'csv';
			}}
		></textarea>
	</div>
</main>

<style>
	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: var(--space-lg);
	}

	textarea {
		height: 100%;
		width: 100%;
		max-width: 400px;
		overflow: auto;
		resize: none;
	}

	.data-inputs {
		height: 80%;
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: var(--space-lg);
	}

	.setting-inputs {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: var(--space-lg);
	}

	.setting-inputs input {
		width: 100px;
	}
</style>
