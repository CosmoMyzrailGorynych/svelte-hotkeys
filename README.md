# Svelte Keyboard shortcuts

[**Demo.**](https://svelte-hotkeys.netlify.app/)

Keyboard shortcuts are a great way to make using your application much quicker. It especially enables your power users to be super quick on common actions.
Think of it as Vim for web applications.

This svelte library makes adding shortcuts easy and also consolidates the display of shortcuts available. (Optional.)

## Get started

`layout.svelte`
```svelte
<script lang="ts">
	import Shortcuts from '@whimsy-engine/svelte-hotkeys';
</script>

<Shortcuts
	options={{
		// default options
		// generateKbd: true // Generate keyboard input element next to use: element.
		// timeout: 1000 // Timeout of keyboard inputs.
	}}
/>
```

`page.svelte`
```svelte
<script>
	import { shortcuts } from '@whimsy-engine/svelte-hotkeys';
</script>

<button class="btn rounded-full border border-gray-400 p-2" use:shortcuts onclick={handleOnClick}
	>Press p to click button</button
>
<div use:shortcuts={{ keys: ['c'] }} tabindex="2">Press C to Focus on me!</div>

<label>
	F to focus on me
	<input use:shortcuts placeholder="Esc to blur." />
</label>

<div use:shortcuts={{ keys: ['z'], modifiers: [['Control', 'Shift'], ['Meta', 'Shift']] }}>
	Press Ctrl+Shift+Z (or Meta+Shift+Z) to focus on me!
</div>

<a use:shortcuts={{ keys: ['a', 's', 'd'] }} href="/about">Ordered keys + links</a>

<div class="flex flex-row">
	<span>Keys pressed:</span>
	{#each keyPressesState as keyPress}
		<kbd>{keyPress}</kbd>
	{/each}
</div>
```

> [!NOTE]
> Keys are handled based on their physical location on a standard QWERTY layout. While this means hotkeys will be the same no matter whether a user has English, German, Russian, or any other layout, the hotkeys might be confusing for DVORAK users and those that prefer other keyboard layouts.
>
> A good design choice would be communicating hotkeys with more than text: for example, with a keyboard illustration with keys and their actions highlighted.

### Using key modifiers (like Ctrl+C)

Modifiers are specified with the `modifiers` option and can be either an array of modifier names (`PressModifiers[]`), ***or*** an array of arrays of modifier keys (`PressModifiers[][]`).

```svelte
<!-- Using PressModifiers[] means that all PressModifiers must be pressed for the trigger to work. -->
<div use:shortcuts={{ keys: ['g'], modifiers: ['Alt', 'Shift']] }}>
	Press Alt+Shift+G to focus on me!
</div>

<!-- Here, using PressModifiers[][] means that any of PressModifiers[] can be used. For example, to create macOS-styled and regular hotkeys: -->
<div use:shortcuts={{ keys: ['z'], modifiers: [['Control', 'Shift'], ['Meta', 'Shift']] }}>
	Press Ctrl+Shift+Z (or Meta+Shift+Z) to focus on me!
</div>
```

You can also set `modifiers` to `false` to explicitly forbid the use of modifier keys:

```svelte
<div use:shortcuts={{ keys: ['a'], modifiers: false }}>
	Press just A to focus on me!
</div>
<div use:shortcuts={{ keys: ['d'] }}>
	Pressing D with any modifier will focus me!
</div>
```

> [!TIP]
> The library also exports a `ShortcutParams` type that you can use, for example, to add a prop to a component that will then be used to make a shortcut inside that component. (The `use:` directive can't be used on components as it requires an explicit HTML tag to apply to.) This is very useful when you have generic components for interactive elements like `Button`, `ComboBox` and others:
>
> ```svelte
> <!-- an oversimplified Button.svelte component -->
> {#if shortcut}
> 	<button use:shortcuts={shortcut}>
> 		{label}
> 	</button>
> {:else}
> 	<button>{label}</button>
> {/if}
>
> <script lang="ts">
> import { shortcuts, ShortcutParams } from '@whimsy-engine/svelte-hotkeys';
>
> let {
> 	label,
> 	shortcut
> }: {
> 	label: string,
> 	shortcut?: ShortcutParams
> } = $props();
> </script>
> ```
> Then you can import this Button component and use it in your markup:
> ```svelte
> <Button shortcut={{keys: ['Space']}} label="Press spacebar!" />
> ```

## Features

Types of action

- Run callback
- Set focus
- Click

Based upon:

- Single key press
- Multi key press
- Ordered key press
- Key Modifiers
