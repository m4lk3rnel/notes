(check the `xpectoLive.frontend` storybook branch)
-  Let's you 'play' with your components in isolation. With Storybook, you don't have to run your entire app for testing a component.
-  you can also write clean documentation for each component. Like [js-doc](https://jsdoc.app/) but better.
-  [[Storybook]] has an in-built test-runner. The [[Storybook]] test runner is powered by [[Jest]] and [[Playwright]]
- `test-storybook` - used for executing storybook's test runner. Provided by the `@storybook/test-runner` package

###### Stories
-  A "story" captures a state of the component.
-  The "Component Story Format" (CSF) is used as a standard for writing stories. It's an open standard that is based on ES6 modules. 
-  `@storybook/csf` - a minimal set of utilities for CSF
```typescript
export const OutlinedButton: Story = {
	args: {
		variant: "outlined"
	}
}
```

###### Component Story Format
-  Stories and metadata are provided by the user using  the ESM format.  Providing metadata example:
```typescript
const meta: Meta<typeof Button> = {
	title: "General/Button",
	component: Button
}

export default meta;
```



