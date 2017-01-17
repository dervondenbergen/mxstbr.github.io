---
published: true
hidden: true
title: "DangerJS: The open source maintainers best friend"
tags: oss javascript
twitter_large: true
image: micro-meta-image.png
custom_excerpt: ""
---

Maintaining a popular open source project is a lot of work, and often users don't realise how much time we maintainers spend triaging incoming Pull Requests. The mental overhead of reviewing a tiny code change can be massive, as the long-term roadmap, test situation, change type (patch/minor/major) and many other factors have to be taken into consideration and make for quite a lot of context switching.

Which is fine, it's the maintainers resonsibility to make sure only good changes are included. That's why there are maintainers, after all.

The issue is that **often the problem with a PR isn't the code**. As a maintainer, I sometimes see new PRs and get ready to review the changes, just to find a failed CI build. I open CI, look at the logs and post a tiny comment saying "Hey, please fix the linting errors.", "Could you add some tests for this change?", "Could you add a CHANGELOG entry please?", "Please remove the `it.only`/`fit` from the tests" etc.

None of these are related to the change at hand and distract from other, more pressing PRs and issues. This happens before the actual code review can take place, so it slows down the whole process and these small back and forths can take a lot of time and brain to complete.

I'm sure if you're a maintainer of an open source project you've experienced this before, and maybe even thought "Wouldn't it be nice to automate this?". I know I have.

## Danger

The good news, that's exactly what Danger does! 🎉

Danger was originally a Ruby project, but ever since [Orta](https://twitter.com/orta) started writing JavaScript professionally he's been hard at work on the [JavaScript port](https://github.com/danger/danger-js), which is now almost up to feature parity.

**Danger checks incoming Pull Requests for meta errors**, including cultural mishaps, common issues and much more. She's not used to check that the code has the right style, she's used to check that the _PR has the right style_. One configures her via a so called "Dangerfile". This is what a simple one looks like:

```javascript
// dangerfile.js

import { danger, fail } from 'danger'

// Check if the CHANGELOG.md file has been edited
const changelogEdited = danger.git.modified_files.includes('CHANGELOG.md')

// Fail the build and post a comment reminding submitters to do so if it wasn't changed
if (!changelogEdited) {
	fail('Please include a CHANGELOG entry. You can find it at [CHANGELOG.md](CHANGELOG.md)')
}
```

See the [DangerJS repo](https://github.com/danger/danger-js) for instructions how to connect this to your CI service, and this is what it ends up looking like:

![Danger commenting on GitHub with the above text](/img/danger-changelog-error.png)

The nice thing about Danger is that it's handcoded, meaning the rules you want to enforce can be general ones (like "Have CHANGELOG entry") but also project specific ones. (like "`ThemeProvider.js` was edited, that might be a SemVer major change!") Stop writing the same repetitive things over and over and over and over...!

To see an example of moderately complex Dangerfiles, take a look at how we use it to make maintaining [`styled-components`](https://github.com/styled-components/styled-components/blob/master/dangerfile.js) easier, or how [@cpojer](https://github.com/cpojer) and co use it to help them maintain [Jest](https://github.com/facebook/jest/blob/master/dangerfile.js).

This project is one I'm very excited about, and I can't wait to see where it goes. It's a good time to be an open source maintainer.
