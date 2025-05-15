<script lang="ts">
	// ===================================================================
	// MAIN APPLICATION COMPONENT
	// Configures and integrates scrollyteller with visualizations
	// ===================================================================
	import Scrollyteller, { loadScrollyteller } from '$lib/index.js';
	import { onMount } from 'svelte';
	
	// ===================================================================
	// SCROLLYTELLER CONFIGURATION
	// Initialize scrollyteller with configuration parameters
	// ===================================================================
	const scrollyData = loadScrollyteller(
		'test', // Scrollyteller name - corresponds to #scrollytellerNAMEtest in CoreMedia
		'u-full', // Class for mount point - u-full makes it full width in Odyssey
		'mark' // Marker prefix - #markNUMBER1 etc. in CoreMedia
	);

	// App state
	let number = 0; // Current section number
	let stProgress; // Scrollyteller progress data
	let previousNumber = 0; // Track previous section for transitions

	// ===================================================================
	// EVENT HANDLERS
	// Handle scrollyteller events for markers and progress
	// ===================================================================
	
	// Handle marker changes (when user scrolls to a new section)
	const onMarker = (marker) => {
		previousNumber = number;
		number = marker.number;
		
		// Apply transition effect when moving between specific sections
		if (previousNumber === 4 && number === 5) {
			// When transitioning from racial to gender distribution
			applyChartTransition();
		} else if (previousNumber === 5 && number === 4) {
			// When transitioning from gender to racial distribution
			applyChartTransition();
		}
	};

	// Track scrolling progress for animations
	const onProgress = (progress) => {
		stProgress = progress;
		
		// Apply subtle animations based on scroll progress
		if (progress && number > 0) {
			// Use onMount to ensure DOM elements are available
			onMount(() => {
				const activeSection = document.querySelector(`.chart-container > div:nth-child(${number * 2 - 1})`);
				if (activeSection) {
					// Apply subtle parallax or opacity changes based on scroll position
					const opacityValue = Math.min(1, Math.max(0.3, 1 - Math.abs(progress.p - 0.5) * 0.5));
					activeSection.style.opacity = opacityValue;
				}
			});
		}
	};
	
	// Function to apply smooth transitions between charts
	function applyChartTransition() {
		onMount(() => {
			// Get all chart containers
			const chartContainers = document.querySelectorAll('.chart-container > div');
			
			// Apply fade out to all containers
			chartContainers.forEach(container => {
				container.style.opacity = 0;
			});
			
			// After short delay, fade in the active container
			setTimeout(() => {
				const activeContainer = document.querySelector(`.chart-container > div:nth-child(${number * 2 - 1})`);
				if (activeContainer) {
					activeContainer.style.opacity = 1;
				}
			}, 300);
		});
	}
</script>

<svelte:head>
	<!-- 
	// ===================================================================
	// FONT IMPORTS
	// Typography resources for the application
	// ===================================================================
	-->
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300..700;1,300..700&family=Figtree:ital,wght@0,300..900;1,300..900&family=Fleur+De+Leah&family=IBM+Plex+Mono:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600;1,700&family=Instrument+Serif:ital@0;1&family=Roboto+Condensed:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">
	<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300..700;1,300..700&family=Figtree:ital,wght@0,300..900;1,300..900&family=Fira+Sans:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Fleur+De+Leah&family=IBM+Plex+Mono:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600;1,700&family=Instrument+Serif:ital@0;1&family=Roboto+Condensed:ital,wght@0,100..900;1,100..900&family=Space+Mono:ital,wght@0,400;0,700;1,400;1,700&display=swap" rel="stylesheet">
</svelte:head>

<!-- 
// ===================================================================
// HEADER SECTION
// Full-screen hero image with title overlay
// ===================================================================
-->
<div class="fullscreen-chapter header-image">
	<div class="header-overlay"></div>
	<div class="header-content">
		<h1 class="main-title">U.S. Exonerations: Lifetimes lost among those wrongfully convicted of murder</h1>
		<h2 class="sub-title"> Maeve Brin // May 15, 2025</h2>
	</div>
</div>

<!-- 
// ===================================================================
// INTRODUCTION CONTENT
// Opening paragraphs before scrollytelling sections
// ===================================================================
-->
<div class="content-container">
	<p class="content-paragraph">Leon Benson slept with his shoes on for the first eight months of his 60-year sentence for the 1998 murder of Kasey Schoen in Indianapolis, Indiana. Benson, 49, spent 24 years of his life in prison for a crime that he did not commit.</p>
	<p class="content-paragraph">"I had the delusion of reprieve and the delusion of reprieve is typically with someone who's getting executed, they believe that at any time, somebody is going to come and just stop this," he said. "And part of my delusion of reprieve was that the system knew it made a mistake and that it was gonna come and get me."</p>
	<div class="image-left">
		<img src="../../src/TestExample/Images/bars.png" alt="Leon Benson" />
	</div>
	<p class="content-paragraph">Benson, a Black man, is the victim of a false witness identification by a White woman who told the police she saw him at the scene of the crime. He sat through two trials, the first resulting in a hung jury, and went to prison for murder in 1999. When he got to the courtroom, he said he quickly realized it was "us versus them."</p>
	<p class="content-paragraph">It was "White people against Black people, the system against the has-nots, the establishment against urban communities, hip hop," he paused, "because everyone that was on my case had a record except for the witness."</p>
</div>

<!-- 
// ===================================================================
// CHAPTER SECTION - BLACK
// Full-screen black background section with white text
// ===================================================================
-->
<div class="fullscreen-chapter black-bg">
	<div class="content-container">
		<p class="content-paragraph">Benson spent years advocating for his freedom. He said he continuously tried to keep himself relevant, by appearing on MSNBC Lockup, maintaining a presence on MySpace and Facebook with the help of one of his prison pen pals, teaching mass incarceration classes alongside college kids, becoming President of Toastmasters club, and helping create a program called "True Self" to change life through literature. He wrote petitions, tried to get a sentence modification, and filed a Habeas Corpus. By 2018, he felt he had exhausted all legal remedies.</p>
		<p class="content-paragraph">He said, however, there was a "paradigm shift" during the pandemic, and following the murder of George Floyd, his case started to gain traction. It was during this time that he connected with Lara Bazelon, a Professor at the University of San Francisco School of Law and the Director of a racial justice clinic in San Francisco, who fought for his eventual release in March of 2023.</p>
		<p class="content-paragraph">Benson's case is one of 1,415 exonerations in the United States since 1989 for a wrongful murder conviction, according to the National Registry of Exonerations. Exonerees like Benson have lost decades of their lives due to flaws in the criminal justice system.</p>
	</div>
</div>

<!-- 
// ===================================================================
// YEARS LOST CHART
// Visualization of years lost to wrongful conviction
// ===================================================================
-->
<div style="min-height:400px; margin: 50px 0"><iframe title="Age when convicted v. years lost" aria-label="Stacked Bars" id="datawrapper-chart-cDXnW" src="https://datawrapper.dwcdn.net/cDXnW/1/" scrolling="no" frameborder="0" style="width: 0; min-width: 100% !important; border: none;" height="400" data-external="1"></iframe><script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(a){if(void 0!==a.data["datawrapper-height"]){var e=document.querySelectorAll("iframe");for(var t in a.data["datawrapper-height"])for(var r,i=0;r=e[i];i++)if(r.contentWindow===a.source){var d=a.data["datawrapper-height"][t]+"px";r.style.height=d}}}))}();
</script></div>

<div class="content-container">
	<p class="content-paragraph">The chart above represents the years lost among those wrongfully convicted of murder. Age of the exoneree when they were released is the number on the far right side of each bar. Exonerees spent an average of 15 years of their lives behind bars prior to their release, with a maximum of 48 years taken from Glynn Simmons who was falsely convicted of a 1975 murder.</p>
	<div style="min-height:510px; margin: 30px 0"><iframe title="Number of Exonerations per State" aria-label="Map" id="datawrapper-chart-BFkx7" src="https://datawrapper.dwcdn.net/BFkx7/1/" scrolling="no" frameborder="0" style="width: 0; min-width: 100% !important; border: none;" height="510" data-external="1"></iframe><script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(a){if(void 0!==a.data["datawrapper-height"]){var e=document.querySelectorAll("iframe");for(var t in a.data["datawrapper-height"])for(var r,i=0;r=e[i];i++)if(r.contentWindow===a.source){var d=a.data["datawrapper-height"][t]+"px";r.style.height=d}}}))}();
</script></div>
<p class="content-paragraph">Prisoners from 46 U.S. states, the District of Columbia, and Puerto Rico have been exonerated since 1989.</p>

</div>

<!-- 
// ===================================================================
// SCROLLYTELLER COMPONENT
// Main scrollytelling section with data visualizations
// ===================================================================
-->
<Scrollyteller
	panels={scrollyData.panels}
	{onMarker}
	{onProgress}
>
	<div class="graphic">
		<div class="chart-container">
			<!-- Render different content based on current section -->
			{#if number === 1}
				<!-- Section 1: NEW CHART - replaced text box with column chart -->
				<div style="width:100%; height:100%; display:flex; align-items:center; justify-content:center;" class="chart-section">
					<iframe title="The Sentences of Exonerees when Convicted" aria-label="Column Chart" id="datawrapper-chart-P7HB7" src="https://datawrapper.dwcdn.net/P7HB7/1/" scrolling="no" frameborder="0" style="width: 0; min-width: 90% !important; border: none;" height="400" data-external="1"></iframe>
					<script type="text/javascript">
						!function(){"use strict";window.addEventListener("message",(function(a){if(void 0!==a.data["datawrapper-height"]){var e=document.querySelectorAll("iframe");for(var t in a.data["datawrapper-height"])for(var r,i=0;r=e[i];i++)if(r.contentWindow===a.source){var d=a.data["datawrapper-height"][t]+"px";r.style.height=d}}}))}();
					</script>
				</div>
			
			{:else if number === 4}
				<!-- Section 4: Race distribution with transition class -->
				<div style="width:100%; height:100%; display:flex; align-items:center; justify-content:center;" class="chart-section race-chart">
					<iframe title="Breakdown of races " aria-label="Donut Chart" id="datawrapper-chart-40U0g" src="https://datawrapper.dwcdn.net/40U0g/1/" scrolling="no" frameborder="0" style="width: 0; min-width: 90% !important; border: none;" height="567" data-external="1"></iframe>
				</div>
			
			{:else if number === 5}
				<!-- Section 5: Gender distribution with transition class -->
				<div style="width:100%; height:100%; display:flex; align-items:center; justify-content:center;" class="chart-section gender-chart">
					<iframe title="Breakdown of Sex" aria-label="Donut Chart" id="datawrapper-chart-lTcTg" src="https://datawrapper.dwcdn.net/lTcTg/3/" scrolling="no" frameborder="0" style="width: 0; min-width: 90% !important; border: none;" height="567" data-external="1"></iframe>
				</div>
			
			{:else}
				<!-- Default/Initial state before scrolling begins -->
				<div class="initial-state">
					<p>Scroll to explore the impact of wrongful convictions</p>
				</div>
			{/if}
		</div>
	</div>
</Scrollyteller>

<!-- 
// ===================================================================
// CONCLUSION SECTION
// Final thoughts and reflections on Leon Benson's story
// ===================================================================
-->
<div class="spacer"></div>
<div class="content-container conclusion-section">
	<p class="content-paragraph">On March 9, 2023, Benson was reading the Art of Respect by James Prince when he heard, "Leon, you're being released effective immediately." He refers to March 9 as his re-birth.</p>
	<p class="content-paragraph">"Hope is in action, as long as you breathe, you can breathe, you can do things, there's hope," the 49 year-old exoneree said. "My selfhood is bigger than my exoneration."</p>
</div>

<!-- 
// ===================================================================
// FOOTER SECTION
// Site footer with contact links and additional information
// ===================================================================
-->
<footer class="site-footer">
	<div class="footer-container">
		<div class="footer-content">
			<h3 class="footer-heading">U.S. Exonerations Project</h3>
			<p class="footer-text">A data visualization on wrongful convictions and the human cost of judicial errors.</p>
			<p class="footer-text">Based on data from the National Registry of Exonerations.</p>
			<p class="footer-text"> Maeve Brin</p>
		</div>
		<div class="footer-links">
			<p class="footer-text">This project is an adaptation of the <a href="https://github.com/abcnews/scrollyteller">Scrollyteller</a> by ABC News.</p>
		</div>
	</div>
</footer>