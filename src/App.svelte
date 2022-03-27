<script>
	import { v4 } from "uuid";

	let notes = [
		{
			id: 1,
			title: "title1",
			content: "content1",
		},
		{
			id: 2,
			title: "title2",
			content: "content2",
		},
	];

	let note = {
		id: "",
		title: "",
		content: "",
	};

	let editStatus = false;

	const cleanNote = () => {
		note = {
			id: "",
			title: "",
			content: "",
		};
	};

	const addNote = () => {
		const newNote = {
			id: v4(),
			title: note.title,
			content: note.content,
		};

		notes = notes.concat(newNote);
		cleanNote();
		console.log(notes);
	};

	const updateNote = () => {
		let updatedNote = {
			id: note.id,
			title: note.title,
			content: note.content,
		};
		const noteIndex = notes.findIndex((n) => n.id === note.id);
		notes[noteIndex] = updatedNote;
		cleanNote();
		editStatus = false;
	};

	const onSubmitHandler = (e) => {
		if (!editStatus) {
			addNote();
		} else {
			updateNote();
		}
	};

	const deleteNote = (id) => {
		notes = notes.filter((note) => note.id !== id);
	};

	const editNote = (noteEdited) => {
		note = noteEdited;
		editStatus = true;
	};
</script>

<main>
	<nav class="blue">
		<div class="nav-wrapper">
			<a href="/" class="brand-logo center">SVE-1</a>
		</div>
	</nav>
	<div class="row">
		<div class="col s12 m6">
			<div class="card blue-grey lighten-5">
				<div class="card-content black-text">
					<span class="card-title"
						>{#if editStatus}Edit {:else} Create {/if} a Note</span
					>
					<form on:submit|preventDefault={onSubmitHandler}>
						<input
							bind:value={note.title}
							id="note-title"
							type="text"
							placeholder="title"
							class="validate"
						/>
						<input
							bind:value={note.content}
							id="note-content"
							rows="3"
							placeholder="content"
							class="validate"
						/>
						<button
							class="waves-effect waves-light btn-small green accent-4"
						>
							{#if !editStatus}Save {:else} Update {/if}
						</button>
					</form>
				</div>
			</div>
		</div>
		<div class="col s12 m6">
			<table class="striped bordered" style="margin: 0.5rem 0 1rem 0">
				<thead>
					<tr>
						<th>Title</th>
						<th>Content</th>
						<th>Edit/Delete</th>
					</tr>
				</thead>
				<tbody>
					{#each notes as note}
						<tr>
							<td>{note.title}</td>
							<td>{note.content}</td>
							<td>
								<button
									on:click={editNote(note)}
									class="waves-effect waves-light btn-small light-blue accent-4"
									>Edit
								</button>
								<button
									on:click={deleteNote(note.id)}
									class="waves-effect waves-light btn-small red accent-4"
									>Delete
								</button>
							</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	</div>
</main>

<style>
	table {
		border: 2px solid #f8f8f8;
	}
</style>
