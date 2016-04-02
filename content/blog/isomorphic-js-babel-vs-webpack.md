+++
date = "2016-04-02T19:07:56+02:00"
draft = false
title = "Isomorphic JS apps: Babel vs Webpack for server-side code"

+++

For quite some time now I've been researching different approaches to creating isomorphic JS applications. There are quite many starter projects on GitHub. Some of them are quite complicated and try to have a solution for every single possible use case. This results in overly complicated setup and steep learning curve to get familiar with the project. Other projects provide only rather basic setup and require a lot of extra work to get them to production like quality.

When it comes to building and bundling client-side code, [Webpack](webpack.github.io) would be quite a common choice. Lots of plugins, extremely flexible setup, loaders for different file types - all this makes Webpack a good choice as a module bundler for client-side code. On the other hand, Webpack configuration tends to become quite complicated as the project grows. Nonetheless, I see handling of client-side code as simple, but not necessarily easy task.

Handling of server-side code for isomorphic apps doesn't seem all that straightforward to me. Many of isomorphic js applications that I've looked at on Github are using Webpack for building and bundling server-side as well as client-side code. And this is something I don't completely agree with.

From one point of view, it's good to have one tool for both client-side and server-side code. Webpack configs for server-side and client-side are still quite different though and tend to get rather complex. But I guess the benefit of using such setup is in having the same approach for handling both client-side and server-side code. What I don't really like though is that server-side code ends up bundled in one file instead of keeping clean folder structure. For client-side there's no option other than building a bundle, but server-side code running in Node doesn't have to end up with lots of Webpack specific code bundled as one file.

An alternative approach that I think is not that common is handling client-side code with Webpack, but using only [Babel](babeljs.io) for the server-side code. This allows server-side code to keep initial folder structure instead of being bundled into one single file. Also with packages like [babel-preset-es2015-node4](https://www.npmjs.com/package/babel-preset-es2015-node4) and [babel-preset-es2015-node5](https://www.npmjs.com/package/babel-preset-es2015-node5) it is possible to transpile from ES6 to ES5 only those features that are missing in either Node 4.x or 5.x. Of course, these presets could be used within Webpack build as well.

Anyway, right now I'm leaning towards using only Babel for server-side code. This approach seems cleaner. But for it to be a fair choice based on good reasons and not a gut feeling there has to be some solid comparisons e.g. of performance and/or code and configuration complexity. I'm not sure I will have time for those. I think if I'll manage to find good solutions for most of the issues related to isomorphic apps (e.g. handling styles, images, i18n and l10n, routing) then I'll just keep using Babel for server-side code and Webpack (with babel-loader) for client-side code.
