<script lang="ts">
	import Scene from '$lib/three/Scene.svelte';
</script>

<section
	class="
		pointer-events-none
		top-0
		left-0
        z-0
		h-[420px]
		min-h-screen
		w-full
		bg-[linear-gradient(to_top,_#000_0%,_#000_3%,_rgba(0,0,0,0.45)_7%,_rgba(0,0,0,0.35)_20%,_transparent_100%)]
		p-10
	"
>
	<div
		class="
      mx-auto
      flex
      min-h-screen
      max-w-7xl
      items-center
      justify-between
      px-6
    "
	>
		<div class="max-w-xl">
			<p class="mb-4 text-sm tracking-[0.3em] text-zinc-400 uppercase">Frontend Engineer</p>

			<h1 class="mb-6 text-5xl leading-tight font-bold md:text-7xl">
				Engineering interfaces that feel alive.
			</h1>

			<p class="mb-8 text-lg text-zinc-400">
				Building immersive digital experiences with motion, interaction, and modern frontend
				technologies.
			</p>

			<div class="flex gap-4">
				<button
					class="
            rounded-full
            bg-white
            px-6
            py-3
            text-black
            transition
            hover:scale-105
          "
				>
					View Projects
				</button>

				<button
					class="
            rounded-full
            border
            border-zinc-700
            px-6
            py-3
            transition
            hover:border-zinc-400
          "
				>
					Resume
				</button>
			</div>
		</div>

		<div class="hidden h-[500px] w-[500px] md:flex">
			<Scene />
		</div>
	</div>
</section>
