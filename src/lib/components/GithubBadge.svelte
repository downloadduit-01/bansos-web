<script lang="ts">
	import { onMount } from 'svelte';

	let { repo = 'wauputr4/bansos' }: { repo?: string } = $props();
	let stars: number | string = $state('-');
	let forks: number | string = $state('-');
	let version: string = $state('v-');

	onMount(() => {
		fetch(`https://api.github.com/repos/${repo}`)
			.then((res) => {
				if (!res.ok) return;
				return res.json();
			})
			.then((data) => {
				if (data) {
					stars = data.stargazers_count;
					forks = data.forks_count;
				}
			})
			.catch(() => {});

		fetch(`https://api.github.com/repos/${repo}/releases/latest`)
			.then((res) => {
				if (!res.ok) return;
				return res.json();
			})
			.then((data) => {
				if (data) version = data.tag_name;
			})
			.catch(() => {});
	});
</script>

<a
	href={`https://github.com/${repo}`}
	target="_blank"
	rel="noopener noreferrer"
	class="repo-link"
	aria-label="GitHub Repository"
>
	<svg class="git-icon" viewBox="0 0 24 24" width="20" height="20" aria-hidden="true">
		<path
			fill="currentColor"
			d="M23.546 10.93L13.067.452c-.604-.603-1.582-.603-2.188 0L8.708 2.627l2.76 2.76c.645-.215 1.379-.07 1.889.441.516.515.658 1.258.438 1.9l2.738 2.738c.64-.22 1.383-.08 1.9.435.812.811.812 2.132 0 2.943-.812.812-2.132.812-2.943 0-.516-.516-.656-1.259-.441-1.9l-2.736-2.736v6.805c.215.21.357.51.357.814 0 .816-.67 1.477-1.477 1.477-.808 0-1.48-.661-1.48-1.477 0-.322.15-.624.382-.833v-6.8c-.22-.208-.373-.508-.373-.83 0-.469.213-.882.542-1.157L5.147 4.18 .453 8.874c-.603.604-.603 1.585 0 2.189l10.478 10.477c.606.604 1.587.604 2.189 0l10.426-10.426c.604-.604.604-1.584 0-2.184"
		/>
	</svg>
	<div class="repo-info">
		<span class="repo-name">{repo}</span>
		<div class="stats-row">
			<span class="stat">{version}</span>
			<span class="stat">★ {stars}</span>
			<span class="stat">⑂ {forks}</span>
		</div>
	</div>
</a>

<style>
	.repo-link {
		display: inline-flex;
		align-items: center;
		gap: 0.6rem;
		border: 1px solid var(--border-color);
		color: var(--text-secondary);
		padding: 0.4rem 0.75rem;
		border-radius: 0.5rem;
		text-decoration: none;
		transition:
			background-color 0.2s,
			color 0.2s;
	}

	.repo-link:hover {
		color: var(--text-primary);
		background: rgba(255, 255, 255, 0.05);
	}

	.git-icon {
		flex-shrink: 0;
	}

	.repo-info {
		display: flex;
		flex-direction: column;
		gap: 0.25rem;
	}

	.repo-name {
		font-size: 0.8rem;
		font-weight: 750;
		line-height: 1;
		color: var(--text-primary);
	}

	.stats-row {
		display: flex;
		align-items: center;
		gap: 0.6rem;
	}

	.stat {
		font-size: 0.75rem;
		font-weight: 600;
		color: var(--text-muted);
		line-height: 1;
	}

	.repo-link:hover .stat {
		color: var(--text-secondary);
	}
</style>
