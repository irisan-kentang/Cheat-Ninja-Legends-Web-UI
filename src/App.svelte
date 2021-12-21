<script>
	let URL = "http://103.176.78.226:10001"
	let loop = "loop_1"
	let uid = ""
	let mission_id = 1
	let iter = 1
	let progress = 0
	let error = 0
	let onProgress = false

	$: progressBarValue = parseInt((progress/iter) * 100) + "%"
	$: progressBarDisplay = "none"

	function start() {
		switch (loop) {
			case "loop_10":
				iter = 10
				break
			case "loop_25":
				iter = 25
				break
			case "loop_50":
				iter = 50
				break
			case "loop_100":
				iter = 100
				break
			default:
				iter = 1
				break
		}

		if (uid == "") {
			alert("uid tidak boleh kosong")
		} else {
			onProgress = true
			error = 0
			progress = 0
			progressBarDisplay = "block"
			doMission()
		}
	}

	function doMission(index = 1) {
		fetch(URL + '/instant_mission', {
				method: "POST",
				headers: {
					"Content-Type": "application/json",
				},
				body: JSON.stringify({
					uid: uid,
					mission_id: mission_id
				}),
			})
				.then((res) => res.json())
				.then((data) => {
					progress = index
					if (index <= iter) {
						doMission(index + 1)
					} else {
						progressBarDisplay = "none"
						onProgress = false
						if (error > 0) {
							alert("Terjadi Kesalahan/Galat")
						} else {
							alert("Instant Mission Selesai")
						}
					}
				})
				.catch((err) => {
					console.log(err)
					progressBarDisplay = "none"
					onProgress = false
					alert("Terjadi Kesalahan/Galat")
				})
	}
</script>

<h1>Cheat Instant Mission Ninja Legends</h1>
<p>Input UID Ninja Legends anda:</p>
<input type="text" placeholder="UID Ninja Legends" bind:value={uid}>
<p>Input Mission ID yang ingin dijalankan (ID 1-10):</p>
<input type="text" placeholder="1" bind:value={mission_id}>
<p>Tentukan banyak misi yang akan dijalankan:</p>
<select bind:value={loop} id="loop">
	<option value="loop_1">1</option>
	<option value="loop_10">10</option>
	<option value="loop_25">25</option>
	<option value="loop_50">50</option>
	<option value="loop_100">100</option>
</select>
<input type="button" value="Start" on:click={start} disabled='{onProgress}'>
<div class="progress-bar" style="--progress-bar-display: {progressBarDisplay}">
	<div class="progress-bar-fill" style="--progress: {progressBarValue}"></div>
</div>

<style>
	.progress-bar {
		display: var(--progress-bar-display);
		width: 300px;
		height: 30px;
		background-color: #e3e3e3;
		border-radius: 5px;
		margin-top: 10px;
	}

	.progress-bar-fill {
		width: var(--progress);
		height: 100%;
		background-color: #27ae60;
		border-radius: 5px;
	}


</style>