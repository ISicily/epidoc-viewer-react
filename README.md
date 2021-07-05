<div>
  <img align="left" valign="center" src="ISicily_english_reduced.jpg?raw=true" alt="isicily logo" height=80" >
  <img align="left" valign="center" src="2258_ox_brand_blue_pos_rect.png?raw=true" alt="oxford logo" height="80"  style="padding-top: 80px" >
  <img align="left" valign="center" src="LOGO_ERC-FLAG_EU_cropped.jpg?raw=true" alt="erc logo" height=80" >
</div>
<br clear="both"/>

<br/>

[![DOI](https://zenodo.org/badge/378974263.svg)](https://zenodo.org/badge/latestdoi/378974263)


# epidoc-viewer-react

Exports a react component  for converting epidoc to Leiden: 

LeidenViewer - a react component that will display rendered Leiden when passed epidoc.

Relies on the convert function of [epidoc-viewer-core](https://github.com/ISicily/epidoc-viewer-core)

The LeidenViewer uses the convert function under the covers, but you can also use the convert function directly without react.

## Usage

```npm install @isicily/epidoc-viewer-react```

OR

```yarn add @isicily/epidoc-viewer-react```

import {LeidenViewer} from ‘@isicily/epidoc-viewer-core’

then use it somewhere...

```<LeidenViewer 
		tei={tei} 
		showInterpreted={false} 
		overridingRules={someRules} 
/>```
  
Where:

- ‘tei’ is the epidoc to be transformed to Leiden.
- ‘showInterpreted’ is a boolean 
	- true shows interpreted, false shows diplomatic
- ‘overridingRules’ is a list of rules to add to the core set, or to override in the core set.  

The rules passed in overridingRules must be an object like so:

```javascript
const yourRules = {
    'w': node => {
        if (node.getAttribute('part') === 'I') {
            const exChild = node.querySelector('ex')
            if (exChild) {
                exChild.append('-')
            }
        } 
    },
    'ex': node => {
        const cert = node.getAttribute('cert')
        node.prepend('('); 
        if (cert === 'low') node.append('?')
        node.append(')')
    },
    'abbr': node => {
        if (node.parentNode.nodeName !== 'expan') node.append('(- - -)')
    }
}
```

Only include rules for those tags you wish to add or override.  You can see the default rules in these three files:

[Interpreted](https://github.com/ISicily/epidoc-viewer-core/blob/master/src/rules.js)

[Diplomatic](https://github.com/ISicily/epidoc-viewer-core/blob/master/src/diplomaticRules.js)

[Shared](https://github.com/ISicily/epidoc-viewer-core/blob/master/src/sharedRules.js)

The shared rules are used in both the diplomatic and interpreted modes.

## Font

The font used in the running [I.Sicily version of this viewer](https://isicily.github.io/epidoc-viewer/) is [New Athena Unicode WOFF](https://apagreekkeys.org/NAUdownload.html).  If  you'd like to use the same font in your app, you'll have to:

1.  Add the following font declaration to your css:

```
@font-face {
  font-family: "NewAthena";
  src: local("NewAthena"),
  url("../fonts/newathu5_7.woff") format("woff");
  font-weight: normal;
}
```

2.  Add the font file to your web app and adjust the 'url' above to point to the location. A copy of the font file is [here](https://github.com/ISicily/epidoc-viewer/blob/master/src/fonts/NAU5_007woff/newathu5_7.woff)

## Updating this repo in NPM

In project directory:

```
npm run build
npm version patch -m "Upgrade to %s"
git push
npm publish --access public
```