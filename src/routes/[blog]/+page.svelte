<script>
	import { page } from '$app/stores';
	import { ValiantRichText, getData } from '@valiantlynx/svelte-rich-text';
	import { pb } from '$lib/utils/api';
	import toast from 'svelte-french-toast';

  const blog = $page.data.blog;

  const saveData = (data) => {
	try {
		console.log(data);
	const datapb = {
    "content_object": data
};
 pb.collection('blogs').update(blog.id, datapb);
 toast.success('Blog post updated successfully');
	} catch (error) {
		console.log(error);
		toast.error('Something went wrong please try again');
	}
  };

</script>


<svelte:head>
	<title>{blog.title} | valiantlynx</title>
	<!-- Canonical Link -->
	<link rel="canonical" href="https://{$page.data.siteName}/" />
	<!-- Author Meta Tag -->
	<meta name="author" content="{$page.data.siteName}" />
	<!--OWN STUFF-->
	<link rel="dns-prefetch" href="https://{$page.data.siteName}" />

	<meta name="apple-mobile-web-app-title" content={$page.data.siteName} />

	<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet" />
	<meta
		name="description"
		content={blog.summary}
		/>
			<!-- Keywords Meta Tag -->
	<meta name="keywords" content="{blog.tags.map((tag) => tag)}" />

	<meta name="mobile-web-app-capable" content="yes" />

	<!-- Open Graph Meta Tags (for social media sharing) -->
	<meta property="og:title" content={blog.title} />

	<meta property="og:type" content="website" />
	<meta property="og:url" content={$page.url.origin} />
	<!--meta property="og:image" content="/twitter-image.png" /-->
	<meta property="og:image:alt" content="{$page.data.siteName} Logo" />
	<meta property="og:site_name" content={$page.data.siteName} />

	<!-- Twitter Meta Tags (for social media sharing) -->
	<meta name="twitter:card" content={blog.image} />
	<meta name="twitter:title" content={blog.title} />
	<meta name="twitter:description" content={blog.summary} />
	<!--meta name="twitter:image" content="/twitter-image.png" /-->

	<!-- Google / Search Engine Tags -->
	<meta itemprop="name" content={blog.title}/>

	<!-- Facebook Meta Tags (for social media sharing) -->
	<meta property="fb:app_id" content={$page.data.siteName} />
	<meta property="fb:admins" content={$page.data.siteName} />
	<meta property="fb:page_id" content={$page.data.siteName} />
	<meta property="fb:site_name" content={$page.data.siteName} />
	<meta property="article:publisher" content={$page.data.siteName} />
	<meta property="article:author" content={$page.data.siteName} />
	<meta property="article:section" content={$page.data.siteName} />
	{#each blog.tags as tag}
<meta property="article:tag" content={tag} />
	{/each}
	<meta property="article:published_time" content={$page.data.siteName} />
	<meta property="article:modified_time" content={$page.data.siteName} />
	<meta property="article:author:first_name" content={$page.data.siteName} />
	<meta property="article:author:last_name" content={$page.data.siteName} />
	<meta property="article:author:username" content={$page.data.siteName} />

	<!-- Schema.org Meta Tags (for SEO) -->
	<meta itemprop="headline" content={blog.title} />
	<meta itemprop="description" content={blog.summary} />
	<!--meta itemprop="image" content="/twitter-image.png" /-->

	{#if $page.data.sites}
		<!-- clarity there is abug in svelte where inside the svript tags i cannot access the variables //! https://stackoverflow.com/questions/63419284/svelte-substitution-in-script-within-sveltehead-->
		{@html `<script type="text/javascript">
			(function (c, l, a, r, i, t, y) {
				c[a] =
					c[a] ||
					function () {
						(c[a].q = c[a].q || []).push(arguments);
					};
				t = l.createElement(r);
				t.async = 1;
				t.src = 'https://www.clarity.ms/tag/' + i;
				y = l.getElementsByTagName(r)[0];
				y.parentNode.insertBefore(t, y);
			})(window, document, 'clarity', 'script', '${$page.data.sites.clarity_tag}');
		</script>`}

		<!-- Google tag (gtag.js) there is abug in svelte where inside the svript tags i cannot access the variables //! https://stackoverflow.com/questions/63419284/svelte-substitution-in-script-within-sveltehead -->
		<!-- Google tag (gtag.js) there is abug in svelte where inside the svript tags i cannot access the variables //! https://stackoverflow.com/questions/63419284/svelte-substitution-in-script-within-sveltehead -->
		<!-- Google tag (gtag.js) there is abug in svelte where inside the svript tags i cannot access the variables //! https://stackoverflow.com/questions/63419284/svelte-substitution-in-script-within-sveltehead -->
		<script
			async
			src="https://www.googletagmanager.com/gtag/js?id={$page.data.sites.google_tag}"
		></script>
		{@html `<script>
			window.dataLayer = window.dataLayer || [];
			function gtag() {
				dataLayer.push(arguments);
			}
			gtag('js', new Date());

			gtag('config', '${$page.data.sites.google_tag}');
		</script>`}

		<!-- Google Adsense -->
		<!-- Google Adsense -->
		<!-- Google Adsense -->
		<script
			async
			src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client={$page.data.sites
				.google_ads_client}"
			crossorigin="anonymous"
		></script>
	{/if}
</svelte:head>



<div class="p-4 md:p-8 lg:p-12 flex justify-center">
	<div class="w-3/4">
		<h1 class="text-2xl md:text-3xl lg:text-4xl font-bold mb-4">
			{blog?.title}
		</h1>
		<img
			src={blog?.image}
			alt={blog?.title}
			class="w-full h-auto rounded-lg mb-4"
		/>
    <p class="text-sm text-accent">By: {blog?.expand?.author.username}</p>
    <p class="text-sm text-accent">Published: {blog?.created}</p>
    <p class="text-sm text-accent">Updated: {blog?.updated}</p>
    <div class="mt-4 progress-primary">
		{#if $page.data.user}
		{#if $page.data.user.id === blog.author}
		<ValiantRichText
		initialData={blog.content_object}
		 />
		<button 
		class="btn btn-primary"
		on:click={()=>{
			const data = getData(); // returns dataBlock[] type
			saveData(data);
		  }}>Save</button>
		{:else}
		<h3 class="text-xl text-accent md:text-2xl lg:text-3xl font-bold mb-4">
			you can not edit this blog post. as you are not the author of this blog post. Create your own blog post <a href="/blogs/new" class="link link-primary">here</a>
		</h3>
		<ValiantRichText initialData={blog.content_object} viewMode={true} />
		{/if}

{:else}
<h3 class="text-xl text-accent md:text-2xl lg:text-3xl font-bold mb-4">
			It is possible to edit this blog post. Please <a href="/login" class="link link-primary">login</a> to edit.
		</h3>
<ValiantRichText initialData={blog.content_object} viewMode={true} />
		{/if}
    </div>

		<p class="text-sm text-secondary">Tags: {blog?.tags}</p>
	
	</div>
</div>

