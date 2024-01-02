# helia-service-worker-gateway

This example shows how to use Helia in a Service Worker to handle fetching and verifying CIDs in the browser.

## Table of Contents

- [Table of Contents](#table-of-contents)
  - [Prerequisites](#prerequisites)
  - [Installation and Running example](#installation-and-running-example)
- [Examples](#examples)
- [Usage](#usage)
- [Demo links](#demo-links)

## Getting Started

### Prerequisites

Make sure you have installed all of the following prerequisites on your development machine:

- Git - [Download & Install Git](https://git-scm.com/downloads). OSX and Linux machines typically have this already installed.
- Node.js - [Download & Install Node.js](https://nodejs.org/en/download/) and the npm package manager.

### Installation and Running example

```console
> npm install
> npm start
```

Now open your browser at `http://localhost:3000`

## Examples

### fetch request to ipfs.io gateway. (what you shouldn't have to do anymore)

```js
// fetch content from ipfs.io gateway (centralized web2.5 gateway for IFPS content)
const response = await fetch('https://ipfs.io/ipfs/bafkreiezuss4xkt5gu256vjccx7vocoksxk77vwmdrpwoumfbbxcy2zowq')

// You can see examples of the below usage in src/components/CidRenderer.tsx
// <img src={URL.createObjectURL(await response.blob())} />
// <video controls autoPlay loop className="center" width="100%"><source src={URL.createObjectURL(await response.blob())} type={contentType} /></video>
// <span>{await res.text()}</span>
```

### request directly to IPFS network using helia

```js
import { heliaFetch } from 'ipfs-shipyard/helia-fetch'

/**
 * A function that returns a helia instance
 * @see https://github.com/ipfs-examples/helia-examples/blob/main/examples/helia-webpack/src/get-helia.js
 */
import { getHelia } from './lib/getHelia'


// fetch content from IPFS directly using helia
const response = await heliaFetch({ path: '/ipfs/bafkreiezuss4xkt5gu256vjccx7vocoksxk77vwmdrpwoumfbbxcy2zowq', helia: getHelia() })

// You can see examples of the below usage in src/components/CidRenderer.tsx
// <img src={URL.createObjectURL(await response.blob())} />
// <video controls autoPlay loop className="center" width="100%"><source src={URL.createObjectURL(await response.blob())} type={contentType} /></video>
// <span>{await res.text()}</span>

```

_For more examples, please refer to the [Documentation](#documentation)_

## Usage

In this example, you will find an example of using helia inside a service worker, that will intercept ipfs path requests (e.g. `/ipfs/*` and `/ipns/*`) and return a `Response` object. This can be used to obtain verified IPFS content instead of using a public IPFS gateway, using helia.

There are two methods of loading content with this demo, each method with their own buttons:

1. Load in-page - This calls heliaFetch directly. NOTE: If you're loading an HTML page (a static website) the service worker must run in order to handle redirecting the loading of content through `heliaFetch`.
2. Load directly / download - This method is processed directly by the service worker, and is essentially an IPFS gateway in the browser via helia in a service worker.

The first thing you will see will look like the below image:

![image](https://user-images.githubusercontent.com/1173416/230510195-ec5dfa8c-fe91-4ecd-b534-caaabf83e11d.png)

As you type in an ip(f|n)s or dnslink path, you will be shown appropriate error messaging:

![image](https://user-images.githubusercontent.com/1173416/230510280-373be0a1-b65b-482b-a41b-9c4a561afe83.png)

![image](https://user-images.githubusercontent.com/1173416/230510324-87139b46-d240-4688-9f27-b75191dce6d1.png)

![image](https://user-images.githubusercontent.com/1173416/230510358-1e380be1-30f6-4426-855f-b0dfc83d2aa3.png)

etc... When your path is valid, you will be shown buttons that will let you load the content:

![image](https://user-images.githubusercontent.com/1173416/230510425-774e2b4e-d1d1-4d95-a651-4c8300360b76.png)


**NOTE:** There is no way to confirm the validity of an `/ipns/` path without actually querying for it, which is not currently done until you attempt to load the content.


## Demo links

### Pre-reqs

You have to visit the [hosted site](https://helia-service-worker-gateway.on.fleek.co/) first, and make sure the SW is loaded. Once it is, the below links should work for you.

Notes:
* Attempting a few refreshes, clearing site data (cache/cookies/sw/indexDb/etc..), etc, may resolve your problem (though may be indicative of issues you can fix with a PR!)
* Some content-types are not *previewable* with certain browsers. If you receive a download prompt you didn't expect: double check the returned content-type and make sure your browser supports previewing that content-type.

### Links

#### Static website and it's nested content

* ipfs.tech/images directory listing page - https://helia-service-worker-gateway.on.fleek.co/ipfs/QmeUdoMyahuQUPHS2odrZEL6yk2HnNfBJ147BeLXsZuqLJ/images
* ipfs.tech/images/images/ipfs-desktop-hex.png - https://helia-service-worker-gateway.on.fleek.co/ipfs/QmeUdoMyahuQUPHS2odrZEL6yk2HnNfBJ147BeLXsZuqLJ/images/ipfs-desktop-hex.png
* ipfs.tech website - https://helia-service-worker-gateway.on.fleek.co/ipfs/QmeUdoMyahuQUPHS2odrZEL6yk2HnNfBJ147BeLXsZuqLJ

#### Single images

* router image - https://helia-service-worker-gateway.on.fleek.co/ipfs/bafkreicafxt3zr4cshf7qteztjzl62ouxqrofu647e44wt7s2iaqjn7bra
* web3.storage logo - https://helia-service-worker-gateway.on.fleek.co/ipfs/bafkreif4ufrfpfcmqn5ltjvmeisgv4k7ykqz2mjygpngtwt4bijxosidqa

#### Videos

* skateboarder stock video - https://helia-service-worker-gateway.on.fleek.co/ipfs/bafkreiezuss4xkt5gu256vjccx7vocoksxk77vwmdrpwoumfbbxcy2zowq
* big buck bunny video - https://helia-service-worker-gateway.on.fleek.co/ipfs/bafybeidsp6fva53dexzjycntiucts57ftecajcn5omzfgjx57pqfy3kwbq

#### IPNS paths

* /ipns/libp2p.io - https://helia-service-worker-gateway.on.fleek.co/ipns/libp2p.io
* /ipns/ipfs.tech - https://helia-service-worker-gateway.on.fleek.co/ipns/ipfs.tech

#### DNSLink paths

* TBD
