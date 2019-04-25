[![Build Status](https://travis-ci.org/oaf-project/oaf-navi.svg?branch=master)](https://travis-ci.org/oaf-project/oaf-navi)
[![Known Vulnerabilities](https://snyk.io/test/github/oaf-project/oaf-navi/badge.svg?targetFile=package.json)](https://snyk.io/test/github/oaf-project/oaf-navi?targetFile=package.json)
[![Greenkeeper badge](https://badges.greenkeeper.io/oaf-project/oaf-navi.svg)](https://greenkeeper.io/)
[![npm](https://img.shields.io/npm/v/oaf-navi.svg)](https://www.npmjs.com/package/oaf-navi)

[![dependencies Status](https://david-dm.org/oaf-project/oaf-navi/status.svg)](https://david-dm.org/oaf-project/oaf-navi)
[![devDependencies Status](https://david-dm.org/oaf-project/oaf-navi/dev-status.svg)](https://david-dm.org/oaf-project/oaf-navi?type=dev)
[![peerDependencies Status](https://david-dm.org/oaf-project/oaf-navi/peer-status.svg)](https://david-dm.org/oaf-project/oaf-navi?type=peer)

# Oaf Navi
An accessible wrapper for [Navi](https://github.com/frontarm/navi)'s router.

Documentation at https://oaf-project.github.io/oaf-navi/

## Features

* Reset scroll and focus after PUSH and REPLACE navigation
* Restore scroll and focus after POP navigation
* Set the page title after navigation
* Announce navigation to users of screen readers
* Hash fragment support

In lieu of more details, see [Oaf React Router](https://github.com/oaf-project/oaf-react-router/blob/master/README.md#features) for now. The features are basically the same.

## Installation

```sh
# yarn
yarn add oaf-navi

# npm
npm install oaf-navi
```

## Basic Usage

```diff
import register from "navi-scripts/register";
import routes from "./routes";
import { createBrowserNavigation } from "navi";
+ import { createBrowserHistory } from "history";
+ import { wrapNavigation } from "oaf-navi";

register({
  ...
  async main() {
-    const navigation = createBrowserNavigation({ routes });
+    const history = createBrowserHistory();
+    const navigation = createBrowserNavigation({ routes, history });
+    await wrapNavigation(history, navigation);
    ...
  }
});
```

## Advanced Usage

```typescript
const settings = {
  announcementsDivId: "announcements",
  primaryFocusTarget: "main h1, [role=main] h1",
  documentTitle: (route: Route) => new Promise(resolve => setTimeout(() => resolve(document.title))),
  // BYO localization
  navigationMessage: (title: string, route: Route, action: Action): string => `Navigated to ${title}.`,
  shouldHandleAction: (previousRoute: Route, nextRoute: Route, action: Action) => true,
  disableAutoScrollRestoration: true,
  announcePageNavigation: true,
  setPageTitle: false,
};

const history = createBrowserHistory();
const navigation = createBrowserNavigation({ routes, history });
await wrapNavigation(history, navigation);
...
```

## See also
* [Oaf Routing](https://github.com/oaf-project/oaf-routing)
* [Oaf Side Effects](https://github.com/oaf-project/oaf-side-effects)
