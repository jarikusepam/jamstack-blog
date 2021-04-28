---
title: Displaying a Deployment Status Badge for your Next.js App on your Github Readme
description: "Some webapp deployment providers, like Netlify, automatically generate badges for displaying the deployment status of your project. Others, like Vercel, don't. This article shows how to easily display a badge for your project regardless of your provider."
date: "2020-11-28"
featured: true
topics: Next.js,Vercel,Github,Badge Generator
recommended: blog-with-next-js-react-material-ui-and-typescript
---

Some webapp deployment providers, like Netlify, automatically generate badges for displaying the deployment status of your project. Others, like Vercel, don't.

It is, however, quite straight forward to implement your own solution for this use case if you use a provider that is integrated with the Github API, like [Vercel](https://vercel.com).

![Github Badges](/images/badges.png)

Let's see how to use this package.

## How to use the NPM package to display a status badge on your Readme

Install _deployment-badge_ with your package manager:

`yarn add deployment-badge` or `npm install --save deployment-badge`

Create an API handler as follows in the directory _pages/api_ of your Next.js project:

```typescript
import type { NextApiRequest, NextApiResponse } from "next";
import deploymentBadgeHandler from "deployment-badge";

const handler = async (
  req: NextApiRequest,
  res: NextApiResponse
): Promise<void> => {
  await deploymentBadgeHandler(req, res, {
    deploymentsUrl: DEPLOYMENTS_URL,
    namedLogo: "vercel",
    env: "Production",
  });
};

export default handler;
```

If you aren't using typescript, simply omit the types above.

The third parameter of `deploymentBadgeHandler` accepts these values as options:

- _deploymentsUrl_: The Github API deployments URL of your project, e.g.
- _namedLogo_: A logo to include in the generated badge. Any name from [Simple Icons](https://simpleicons.org/) can be chosen. Can be omitted.
- _env_: The environment for which to generate the badge. Can be omitted, default is _Production_

This handler will generate JSON responses that can be used by [Shields.io](https://shields.io), from where they will be added to the README.md:

```markdown
[![Deployment Status](https://img.shields.io/endpoint?url=https://devx.sh/api/deployment)](https://devx.sh)
```

Replace the URL above with the URL of your deployed handler.
