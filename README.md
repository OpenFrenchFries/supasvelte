# SupaSvelteKit ⚡

> 🌟 Where Svelte's elegance meets Supabase's might! 🌟

SupaSvelteKit is a library designed to seamlessly integrate SvelteKit with Supabase, providing developers with a powerful toolkit to build dynamic web applications.

## 🎉 Features

- **Authentication** 🔐: Easily integrate Supabase authentication with SvelteKit.
- **Realtime** ⏱️: Simplified multi-user realtime updates with Svelte stores.
- **Storage** 📦: Manage your Supabase storage with Svelte components.
- ... and more to come! Stay tuned and feel free to suggest or contribute new features!

## 🛠 Installation

```bash
npm install supasveltekit
```

## 🚀 Quick Start

1. Initialize your Supabase client:  
```src/lib/supabase.ts```
```ts
import { PUBLIC_SUPABASE_KEY, PUBLIC_SUPABASE_URL } from '$env/static/public';
import { createClient } from '@supabase/supabase-js';

export const supabaseUrl = PUBLIC_SUPABASE_URL;
export const supabaseKey = PUBLIC_SUPABASE_KEY;

export const supabase = createClient(supabaseUrl, supabaseKey);
```

2. Use SupaSvelteKit components in your SvelteKit app:  
```src/routes/+page.svelte```
```svelte
<script lang="ts">
	import { supabase } from '$lib/supabase';
	import { SupabaseApp, Authenticated, Unauthenticated, RealtimePresence, BucketFilesList } from 'supasveltekit';
</script>

<SupabaseApp {supabase}>
	<Authenticated let:session let:signOut>
		<h1>Welcome, {session?.user?.identities?.[0]?.identity_data?.email}!</h1>
		<button on:click={() => signOut()}>Sign Out</button>

		<BucketFilesList bucketName="test-bucket" path="public/" let:bucketFiles let:error>
			{#if error !== null}
				<div>{error}</div>
			{/if}
			<ul>
				{#each bucketFiles as file}
					<li>
						{file.name}
					</li>
				{/each}
			</ul>
		</BucketFilesList>

		<RealtimePresence channelName="my-channel">
			<div slot="default" let:state let:error let:realtime let:channel let:updateStatus>
				{#if error}
					<p>Error: {error.message}</p>
				{:else if !state}
					<p>Loading...</p>
				{:else}
					<p>Connected to channel {channel?.name}</p>
					<p>Number of users: {state?.count}</p>
					<button on:click={() => updateStatus({ status: 'online' })}>Go online</button>
					<button on:click={() => updateStatus({ status: 'offline' })}>Go offline</button>
				{/if}
			</div>
		</RealtimePresence>
	</Authenticated>

	<Unauthenticated>
		<h1>Sign in to continue</h1>
		<button on:click={() => signIn()}>Sign In</button>
	</Unauthenticated>
</SupabaseApp>
```

## 📚 Documentation

For detailed documentation, usage guides, and API references, dive into [our documentation site](http://SupaSvelteKit.openfrenchfries.com/).

## 📖 Examples

Explore hands-on examples to get a feel for how SupaSvelteKit enhances your projects:

- [Basic Todo App](https://github.com/OpenFrenchFries/supasveltekit-example-todo)
- [Authentication Demo](https://github.com/orgs/OpenFrenchFries/repositories)
- ... and more examples coming soon! If you've built something cool with SupaSvelteKit, let us know!

## 💪 Contributing

Join the SupaSvelteKit journey! 🌍 Whether you're fixing bugs, suggesting enhancements, or enriching the documentation, every contribution counts. Dive into our [CONTRIBUTING.md](.github/CONTRIBUTING.md) to get started.

## 📜 License

Freedom with responsibility! SupaSvelteKit is [MIT licensed](LICENSE), ensuring open use with acknowledgment.

## 🙌 Acknowledgements

A big shoutout 📣:

- To the Svelte and Supabase communities for lighting the way with invaluable resources and support.
- To every coder, contributor, and coffee mug that's been part of this journey. ☕

---

Crafted with 🧡 by [OpenFrenchFries](https://github.com/OpenFrenchFries) and the amazing contributors. [Join us!](.github/CONTRIBUTING.md)
