<script>
	import { onMount } from "svelte";

	let movies = [];

	let pagination = {};

	const getallmovies = async (take, skip) => {
		let result = await fetch(
			"http://localhost:3000/movies?take=" + take + "&skip=" + skip 
		);
		
		result = await result.json();

		movies = result.data;
		console.log("the movies", movies) ;
		pagination = result.pagination;
	};
	onMount(getallmovies);
</script>




<h2 style="text-align: center;text-decoration:underline ; padding: 20px;"> La liste des Films </h2>
<main class="container mt-4">
	<div class="row">
		<table class="table table-light">
			<thead>
				<tr>
					<th scope="col">Titre</th>
					<th scope="col">Genres</th>
					<th scope="col">Year</th>
				</tr>
			</thead>

			<tbody>
				{#each movies as movie}
					<tr>
						<td>{movie.title}</td>
						<td>{movie.genres.join(", ")}</td>
						<td>{movie.year}</td>
					</tr>
				{/each}
			</tbody>
		</table>

		<nav aria-label="Page navigation">
			<ul class="pagination">
				{#each { length: pagination.count/pagination.take } as b, i}
					<li
						on:click={
							getallmovies(
							pagination.take,
							i * pagination.take
						)}
						class="page-item {i == pagination.skip / pagination.take
							? 'active'
							: ''}"
					>
						<a class="page-link">{i + 1}</a>
					</li>
				{/each}
			</ul>
		</nav>
	</div>
</main>
