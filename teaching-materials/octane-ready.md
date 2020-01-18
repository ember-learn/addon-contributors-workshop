# Ember Octane for Addons

Octane is Ember's first edition! It introduces some exciting new features and patterns to Ember apps and addons, starting in Ember v3.15. If you are an addon author, maintainer, or contributor, there are a few things you need to know.

## Make a decision - Octane-ready vs 100% Octane?

If your addon is used by apps that are versions less than 3.15, you should work to make your addon "Octane ready," which means it is backwards compatible for use in apps v3.4 or higher, and also works great in Octane apps (3.15 or higher).
"Octane ready" steps are recommended for all public, open source addons that existed before 3.15.

If your addon does not need to work for older versions of Ember, such as an addon that is used privately by your company and you use all Octane apps, you can make a 100% Octane addon.

### A backwards-compatible, Octane-ready app:

- Works in Ember apps that are within the active LTS (long term support) range, i.e. 3.4 or later 
- Does _not_ use Glimmer Components (`import Component from '@glimmer/component'`)
- Does _not_ use `@tracked` (`import { tracked } from '@glimmer/tracking'`)
- Does _not_ use template-only components (every component `.hbs` file must have a corresponding `.js` file
- Does _not_ use template co-location. This means that all templates should go in `addon/templates/components/` and `app/templates/components`, not `addon/components` and `app/components`
- May optionally use jQuery, if it is installed with the `@ember/jquery` package and used like `import $ from '@ember/jquery'`
- Does _not_ use `this.$` or `Ember.$`. These are known as jQuery integration APIs.
- Has `ember-try.js` tests for all active LTS (long term support) versions of Ember, plus versions 3.13 and higher
- If it uses Observers, they are marked `aysnc: true`, and the addon includes the async observers polyfill
- May optionally use the latest versions of `ember-source`, `ember/data`, etc. in its `package.json` 

To be very clear, if you use any features from `@glimmer`, template-only components, or template co-location, apps with versions <3.15 will not be able to use your addon!

### A 100% Octane addon

- Only works in apps with versions 3.15 or higher
- Has the same configuration, packages, and features as an Octane app
- May optionally use `@glimmer/component` and `@glimmer/tracking`
- May optionally use jQuery, if it is installed with the `@ember/jquery` package and used like `import $ from '@ember/jquery'`
- Has `ember-try.js` tests for versions 3.15 on up, only. Tests for lower versions will not pass.

## Optional syntax and linting improvements

There are some updates that you can safely make in addons that support all active LTS versions of Ember.
These can make the addon codebase feel much more Octane-y without breaking the experience for users who are on older versions of Ember.

You can:
- Run the ES5 getters codemod (only if you haven't already run the native classes codemod)
- Run the Native Classes codemod, and write components and other files using Native Class JavaScript syntax
- Use `this.` in your templates, to get rid of linting warnings about "no-implicit-this."
- Run Angle Brackets codemods. If the addon still supports version 3.3 or earlier, install the polyfill.
