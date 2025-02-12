<script lang="ts">
	import ProjectsPopup from './ProjectsPopup.svelte';
	import { clickOutside } from '$lib/clickOutside';
	import Icon from '$lib/components/Icon.svelte';
	import type { Project, ProjectService } from '$lib/backend/projects';

	export let project: Project | undefined;
	export let projectService: ProjectService;

	let popup: ProjectsPopup;
	let visible: boolean = false;
</script>

<div class="wrapper">
	<div
		class="relative"
		use:clickOutside={{
			handler: () => {
				popup.hide();
				visible = false;
			},
			enabled: visible
		}}
	>
		<button
			class="button"
			on:click={(e) => {
				visible = popup.toggle();
				e.preventDefault();
			}}
		>
			<span class="button__label text-base-14 text-bold">{project?.title}</span>
			<div class="button__icon">
				<Icon name="select-chevron" />
			</div>
		</button>
		<ProjectsPopup bind:this={popup} {projectService} />
	</div>
</div>

<style lang="postcss">
	.wrapper {
		margin-top: var(--space-10);
		margin-bottom: var(--space-16);
	}

	.button {
		display: flex;
		width: 100%;
		padding: var(--space-12);
		border-radius: var(--radius-m);

		background-color: var(--clr-theme-container-pale);

		align-items: center;
		justify-content: space-between;

		transition: background-color var(--transition-fast);

		&:focus,
		&:hover {
			background-color: color-mix(
				in srgb,
				var(--clr-theme-container-light),
				var(--clr-core-ntrl-50) 20%
			);

			& .button__icon {
				opacity: 0.4;
			}
		}
	}

	.button__label {
		flex-grow: 1;
		color: var(--clr-theme-scale-ntrl-0);
		text-align: left;
	}

	.button__icon {
		color: var(--clr-theme-scale-ntrl-0);
		opacity: 0.3;
	}
</style>
