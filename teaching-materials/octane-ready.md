# Ember Octane for Addons

Octane is Ember's first edition! It introduces some exciting new features and patterns to Ember apps and addons, starting in Ember v3.15. If you are an addon author, maintainer, or contributor, there are a few things you need to know.

## Make a decision - Octane-ready vs 100% Octane?

If your addon is used by apps that are versions less than 3.13, you should work to make your addon "Octane ready," to continue to work in those older apps but also work in apps that have adopted Octane.
"Octane ready" steps are recommended for all public, open source addons that existed before the official Octane release in 3.15.

If your addon does not need to work for older versions of Ember, such as an addon that is used privately by your company and you use all Octane apps, you can make a 100% Octane addon.

### A backwards-compatible, Octane-ready app:

- Works in Ember apps that are within the active LTS (long term support) range, i.e. 3.12 and 3.16.
- Does _not_ use `@tracked` (`import { tracked } from '@glimmer/tracking'`)
- Does _not_ use template-only components (every component `.hbs` file must have a corresponding `.js` file
- Does _not_ use template co-location without the `setComponentTemplate` polyfill. This means that all templates should go in `addon/templates/components/` and `app/templates/components`, not `addon/components` and `app/components`
- May optionally use jQuery, if it is installed with the `@ember/jquery` package and used like `import $ from '@ember/jquery'`
- Does _not_ use `this.$` or `Ember.$`. These are known as jQuery integration APIs.
- Has `ember-try.js` tests for all active LTS (long term support) versions of Ember, plus the current release.
- If it uses Observers, they are marked `sync: false`, and the addon includes the async observers polyfill
- May optionally use the latest versions of `ember-source`, `ember/data`, etc. in its `package.json` 

To be very clear, if you use `@tracked` from `@glimmer`, template-only components, or template co-location, apps with versions <3.15 will not be able to use your addon!

### A 100% Octane addon

- Only works in apps with versions 3.13 or higher
- Has the same configuration, packages, and features as an Octane app
- May optionally use `@glimmer/tracking`
- Does not use `computed`s
- Does not provide `@ember/components`. Uses `@glimmer/component` if creating components.
- Does not use mixins.
- Uses colocation.
- Does not use observers.
- Does not use `get` and `set` unless needed to interact with an older-style addon.
- Does _not_ use `this.$` or `Ember.$`. These are known as jQuery integration APIs.
- May optionally use jQuery, if it is installed with the `@ember/jquery` package and used like `import $ from '@ember/jquery'`
- Has `ember-try.js` tests for versions 3.13 on up, only. Tests for lower versions will not pass if using `@glimmer/tracking` (no polyfill) or other features when not providing polyfills.

## Optional syntax and linting improvements

There are some updates that you can safely make in addons that support all active LTS versions of Ember.
These can make the addon codebase feel much more Octane-y without breaking the experience for users who are on older versions of Ember.

You can:
- Run the ES5 getters codemod (only if you haven't already run the native classes codemod)
- Run the Native Classes codemod, and write components and other files using Native Class JavaScript syntax
- Use `this.` in your templates, to get rid of linting warnings about "no-implicit-this."
- Run Angle Brackets codemods. If the addon still supports version 3.3 or earlier, install the polyfill.

## What is a polyfill?

A polyfill is a general term for some code that allows you to use a feature found in a later version of a library than the version you are using.
Polyfills exist all over the web.
For example, let's say a web developer wants to use a feature supported by Firefox, but it isn't yet available in Safari. If there's a polyfill, the developer can install it in their app, and both Firefox and Safari users have a good experience.

So why are polyfills important in Ember? One of the highlights of the Ember addon ecosystem is that many addons are backwards compatible! Many addons work fine even if the addon is on a higher version of Ember than the app that is using it. An addon may be using features that aren't available in an earlier version of Ember, but yet it is still backwards-compatible thanks to polyfills.

### Using Polyfills

To install a polyfill:

```
npm install polyfill-name
```

It is importany that polyfills go in the `dependencies` section of your package.json, not `devDependencies`! Things in `devDependencies` aren't available to the apps that use your addon.

There are many free, open source polyfills, but it is up to the addon contributors to install them, based on the versions of Ember apps that they aim to support.

One way to tell that your addon needs a polyfill is by using the `ember-try` feature of addon testing suites. `ember-try` will run your addon tests in other versions of Ember. If a particular scenario (i.e. version) fails, that is your clue that you may need to install a polyfill.

<!-- Some common polyfills include:

TODO -->
