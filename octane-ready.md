# Octane Ready

- Observers need to be `aysnc: true`, and include the async observers polyfill
- Cannot use jQuery integration apis like `this.$`, `Ember.$`. Install jquery explicitly if it's needed
- testing in Ember Try agains 3.13 or higher, plus all LTS

## Things you can't use

- No glimmer components
- No tracked
- Not allowed to do template-only components
- No co-location (unless there is a polyfill before EmberConf)

# Some linting conversions

- native classes are fine
- no implicit this
- Angle Brackets codemods (make sure the addon has dropped support for 3.3, which is when it was added. Or, add polyfill)
- ES5 getters codemod (can only do this one if you haven't already run native classes codemod)
- You can run the Native Classes codemod if preferred
