# Jest expo with Sentry reproduction

Reproduction to showcase that the `transformIgnorePatterns` of the Jext Expo preset
is not up-to-date for use with `@sentry/react-native`.

How I setup this reproduction:
1. Create expo app with `npx create-expo-app@latest`
2. Installed sentry with `npx expo install @sentry/react-native`
3. Added Sentry import to `ThemedText.tsx` and called `addBreadcrumb`

Now the tests won't run when exectued with: `npm run test`. The following error
is shown:

```
Jest encountered an unexpected token

Jest failed to parse a file. This happens e.g. when your code or its dependencies use non-standard JavaScript syntax, or when Jest is not configured to support such syntax.

Out of the box Jest supports Babel, which will be used to transform your files into valid JS based on your Babel configuration.

By default "node_modules" folder is ignored by transformers.

Here's what you can do:
• If you are trying to use ECMAScript Modules, see https://jestjs.io/docs/ecmascript-modules for how to enable it.
• If you are trying to use TypeScript, see https://jestjs.io/docs/getting-started#using-typescript
• To have some of your "node_modules" files transformed, you can specify a custom "transformIgnorePatterns" in your config.
• If you need a custom transformation specify a "transform" option in your config.
• If you simply want to mock your non-JS modules (e.g. binary assets) you can stub them out with the "moduleNameMapper" config option.

You'll find more details and examples of these config options in the docs:
https://jestjs.io/docs/configuration
For information about custom transformations, see:
https://jestjs.io/docs/code-transformation

Details:

/Users/koencastermans/Documents/git/jest-expo-repro/node_modules/@sentry/react-native/dist/js/index.js:1
({"Object.<anonymous>":function(module,exports,require,__dirname,__filename,jest){export { addGlobalEventProcessor, addBreadcrumb, captureException, captureEvent, captureMessage, getHubFromCarrier, getCurrentHub, Hub, Scope, setContext, setExtra, setExtras, setTag, setTags, setUser, startTransaction,
                                                                                 ^^^^^^

SyntaxError: Unexpected token 'export'

1 | import { Text, type TextProps, StyleSheet } from 'react-native'
> 2 | import * as Sentry from '@sentry/react-native'
   | ^
3 |
4 | import { useThemeColor } from '@/hooks/useThemeColor'
5 |

at Runtime.createScriptFromCode (node_modules/jest-runtime/build/index.js:1505:14)
at Object.require (components/ThemedText.tsx:2:1)
at Object.require (components/__tests__/ThemedText-test.tsx:4:1)
```

In the [Unit testing with Jest](https://docs.expo.dev/develop/unit-testing/#installation-and-configuration)
docs of Expo an up-to-date `transformIgnorePatterns` is available which includes the
new way of using Sentry in Expo:

```
  "transformIgnorePatterns": [
    "node_modules/(?!((jest-)?react-native|@react-native(-community)?)|expo(nent)?|@expo(nent)?/.*|@expo-google-fonts/.*|react-navigation|@react-navigation/.*|@sentry/react-native|native-base|react-native-svg)"
  ]
```

When we add this to `jest` object in the `package.json` then the tests start
working again.
