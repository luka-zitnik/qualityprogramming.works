---
layout: post
title: Introducing Frontend Development Practices Into Your PHP
subtitle: Are frontend JavaScript and PHP development practices comparable?
date: 2018-05-02 00:00:00 +0200
background: /images/alexandre-godreau-203580-unsplash.jpg
---

Are there any frontend programming practices PHP guys are missing out? Could they look up to the frontend guys and make their development more enjoyable or more productive?

The answer is yes. They are missing out everything. They should switch to JavaScript.

All right! You got me! I'm just fooling around! 😉

Actually, this was a real question I was asked by my manager, twice. And twice I could remember only there must be some prominent features of [Composer](https://getcomposer.org/) that are missing in [Npm](https://docs.npmjs.com/) and [Yarn](https://yarnpkg.com/en/). They could imply some differences in development practices. Only, to me, it's the other way round. Like, the `npm publish` accepts anything from a working copy, which may differ from what's tagged, while [packagist.org](https://packagist.org/) can publish proactively when there's a new tag in the repo. Or that Composer lets you write custom commands as static methods and helps you with some nice contextual event objects along the way. We drew out a conclusion that both ecosystems are mature enought and close toghether enough to look up to each other all the time. I am wondering if there's more, so here's another try.

For now, let's not talk about language features, as a more sensible thig to do. And let's not talk about programming patterns neither. They are task-specific after all. So that leaves tooling and libraries.

## Caches vs Watchers

PHP does involve some compiling sometimes. Routing configurations, service container configurations, templates and others imply some execution steps i.e. PHP statements. These are validated, generated, cached and read from cache with each app execution while developing the app. In my case that means I have to wait 3 extra seconds when I change routes, 10 extra seconds when I change service constructors and 3 extra seconds when I change templates.

In comparison, watching for file changes and compiling in the background, like what I've seen in [Wecpack](https://webpack.js.org/), seems a more reasonable solution. If there's no obvious distinction between the build and the runtime, what's percieved is that the app behaves slugishly in development.

This is different from disabling the cache altoghether. But if it could be possible, which I don't see as an option with Symfony's service container, it's questionable whether the app would perform any better without it.

## Live & Hot Reloading

Where's PHP there are usually also some frontend assets to manage and integration bugs to fix. Last year live reloads went mainstream in the world of PHP development with [Webpack Encore](https://symfony.com/doc/current/frontend.html) and [Mix](https://laravel.com/docs/5.4/mix), both wrappers around [Webpack](https://webpack.js.org/), an alien. There are alternatives for users of [Grunt](https://gruntjs.com/) and [Gulp](https://gulpjs.com/). There's also a [native implementation](https://github.com/RickySu/php-livereload). All in all, live and hot reloading are available and, they belong to you.

## Test Watchers

[Jest](https://facebook.github.io/jest/en/) has spoiled the frontend community with its [intelligent watcher](https://facebook.github.io/jest/docs/en/cli.html#watch). [Mocha](https://github.com/mochajs/mocha) and other test runners possibly as much. In PHP, [Peridot](http://peridot-php.github.io/) is up to the challenge, and beggining last year, there's [a watcher](https://github.com/spatie/phpunit-watcher) for PHPUnit tests.

## Afterword

So, there it goes. Watchers everywhere. I wanted to talk a little about REPLs, but later decided they don't make the cut. I hope you like the read. Please let me know your thoughts. Don't be shy!

## Resources

- [Scripts - Composer](https://getcomposer.org/doc/articles/scripts.md)
- [How Symfony Builds the Container > Journey to the Center of Symfony: The Dependency Injection Container \| KnpUniversity](https://knpuniversity.com/screencast/symfony-journey-di/symfony-builds-the-container)
- [Creating and Using Templates (Symfony Docs)](https://symfony.com/doc/current/templating.html#twig-template-caching)
- [Twig/Environment.php at 1dcb150c94188a550c779c17dc684f43210c2910 · twigphp/Twig](https://github.com/twigphp/Twig/blob/1dcb150c94188a550c779c17dc684f43210c2910/lib/Twig/Environment.php#L348)
- [Watch and WatchOptions](https://webpack.js.org/configuration/watch/)
