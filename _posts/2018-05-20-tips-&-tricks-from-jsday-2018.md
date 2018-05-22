---
layout: post
title: Tips & Tricks From JSDay 2018
subtitle: Our report from this year's JSDay conference in Verona, Italy. Find out which talks we've highlighted and consider the best, and replenish your toolbox with tips & tricks from the trenches of JavaScript.
date: 2018-05-20 00:00:00 +0000
background: /images/alexandre-godreau-203580-unsplash.jpg
---

When speakers try to explain why do people come to conferences to listen to them, they say they come to be inspired. And that belief seems to be ubiquitous. People with same interests, skilled speakers, new insights, reviews of the past technology and anticipations of the future, new city, its culture and everything it holds, all have potential to stimulate visitors' thoughts.

Talks sometimes cover forming technology, but more often it's about the state of the art. In the ever evolving world of web programming it's most rewarding to learn new tips and tricks which you can take back home with you and turn into immediate benefit, or start off with your decision-making from a more informed standpoint.

Here are the best of the best, in the order of their timeslots.

## Zack Argyle, Pinterest ❤️ Mobile Web

First part of the talk focused on helping you make some requirements considerations. You are a website owner and you are wondering should you go mobile web or upsell a mobile app. You might prefer the former if:

- The content should be discoverable with web search engines
- Your target audience is discouraged from becoming users because your website runs too slowly on their devices
- Your target audience prefers not to install native app

You decided to go mobile web, now you wonder whether you should make your website responsive or make a mobile version of the website.
The speaker seems to suggest that you should not build a mobile version if:

- Features on desktop break mobile
- You don't have sufficient capacity (you have less than 20 web developers) to build two separate products
- Optimization for touch surfaces is not required
- You don't need additional surface for experimentation
- You are willing to deal with logical complexity involved with responsiveness

Another part of the talk was focused on mobile web performance optimizations. Pinterest split their JS code into Webpack chunks, a `vendor` that contains external dependencies, an `entry` that contains common code and multiple `async` chunks that contain code pertaining to individual routes. Analyzing the chunks with *Webpack Bundle Analyzer* helped them to remove duplicate code from the async chunks and consequently cut their size to 17-18KB. Other techniques they used are virtualized grid layout with *Masonry grid*, Redux data normalization (which allowed them to display data from the app store before complementary data is available) with some help from *normalizr* and Webpack chunks precaching with *Service Workers*.

The improvements they got are the improvements you might expect as well:

<table class="table">
  <thead>
    <tr>
      <th>Metric</th>
      <th>Old Mobile Web</th>
      <th>New Mobile Web</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>TTFP (Time to first paint)</td>
      <td>4.2s</td>
      <td>1.8s</td>
    </tr>
    <tr>
      <td>TTI (Time to interactive)</td>
      <td>23s</td>
      <td>5.6s</td>
    </tr>
    <tr>
      <td>JS Bundle Size</td>
      <td>620KB</td>
      <td>150KB</td>
    </tr>
    <tr>
      <td>CSS Bundle Size</td>
      <td>150KB</td>
      <td>6KB</td>
    </tr>
  </tbody>
</table>

And consequently, their usage statistics ascended as well:

<table class="table">
  <thead>
    <tr>
      <th>Metric</th>
      <th>Mweb Increase</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Pin impressions</td>
      <td>+299% y/y</td>
    </tr>
    <tr>
      <td>Ad Impressions</td>
      <td>+290% y/y</td>
    </tr>
    <tr>
      <td>Signups</td>
      <td>+704% y/y</td>
    </tr>
    <tr>
      <td>Logins</td>
      <td>+172% y/y</td>
    </tr>
    <tr>
      <td>Repins</td>
      <td>+279% y/y</td>
    </tr>
    <tr>
      <td>Time Spent</td>
      <td>+289% y/y</td>
    </tr>
  </tbody>
</table>

## Matteo Collina And Luca Maraschi, The Node.js Performance Workshop

A real workshop where you find out how it feels to be given best tools, an app and a performance goal to reach. It's not accidental that this workshop is highlighted and it also, like the first one, starts with a typical scenario. You have your performance goal, you need to measure how your application behaves in simulated environment and discover and fix any potential bottlenecks of your application until the goal is reached. The debugging was done in cycles, measure (with *AutoCannon*), identify bottleneck (with *Node Clinic*), optimize, rinse and repeat, with each cycle removing another concealed bottleneck. First optimization was made by removing a premature optimization that actually affected performance in negative way, two more were made after discovering *v8 deoptimizations* took place and another one was made after discovering a large flame chart block belonging to the http framework that was being used, by replacing the framework with *Fastify*.

## William Durand, Server Side Rendering From The Trenches

Complementing the Pinterest story, this talk led into the topic of SSR, explaining difficulties it imposes and providing plenty of resources for further investigation. The difficulties seem they can be grouped in two categories.

- Those involved with different execution environment, which should be resolvable with headless browser solutions like *Prerender*
  - React's `renderToString()` holds the event loop, requiring more servers to run, raising expenses
  - JS errors cause 500 responses
  - Errors that manifest in only one of the environments
  - Bugs in the *isomporphic* libraries
- Difficulties related to app data
  - Synchronization between the client and the server

The talk concluded by prompting you to reconsider whether you need SSR or not, preferences towards SSR being mainly faster TTFP and SEO (as e.g. `react-router` seems to confuse Google crawler).

## Brian Holt, 10KB or Bust: The Delicate Power of Webpack And Babel

A perfectly clear talk with a myriad of tips and tricks, just as the speaker promised. First things first, Brian made sure we know what we are doing when we optimize for slower networks and then showed us what to take care of to go below 10KB of code. Here's a checklist for you.

- Babel's `preset-env` by which Babel progressively transpiles less of the application code as time goes on and consequently makes its bundles smaller and the website faster
- Native support for *ES modules* with which you could decrease transpiling even further in browsers that support them (but according to research done by Nir Noy and presented in his talk “ES Modules: The next revolution?”, it turns out they are currently inferior to Webpack bundles)
- Tree shaking, bundling only parts of libraries that your code actually uses, specifically using libraries that can be tree shook, like `lodash-es`, instead of `lodash`
- Babel's `"useBuiltins": "usage"` that imports only the polyfills for the ES features used in the app (This is actually an upcoming configuration for `preset-env` in Babel 7, so the polyfills included are also only those necessary for the target browsers.)
- Babel's loose mode, which excludes some edge cases for ES6 class support (Btw, do you find this similar to some lodash implementations being faster than native ones?)
- `NODE_ENV="PRODUCTION"` in build command, which excludes development features, like error messages and source maps, from the production build
- Code splitting (also covered in the Pinterest talk), the only way that can get you below 10KB of code (Webpack makes this easy with its `import()` function. Wherever you call the function, it splits the code.)
- *ModuleConcatenationPlugin* (Webpack) / *Rollup* / *Closure compiler*, with their feature called *scope hoising* make smaller bundles (it may reduce the bundle size in half) and make code run faster
- Image skeletons, perceived performance optimization, where you load replacement images so your website becomes interactive before the actual larger images are downloaded – these include progressive polygon approximations, two-color outlines, SVG line animations, generic placeholders, dominant colors and blur-ups
- Server-side TTFB optimizations and pre-packing serverless functions with *Azure Functions Pack*
- Compressing responses (with *Brotli* if possible, which brought 90% initial page load improvement to LinkedIn)

Last part of the talk showed futuristic topics of code optimization with *Prepack*, Angular's *AOT* compilation, web components bundling instead of frameworks with *Svelte* and *Angular Ivy*, binary templates with *Svelte* and *Glimmer* but also what they mean in raw numbers in a Hacker news clone comparison example:

<table class="table">
  <thead>
    <tr>
      <th>Name</th>
      <th>3G TTI (SEC)</th>
      <th>2G TTI (SEC)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>PREACT</td>
      <td>1.5</td>
      <td>1.92</td>
    </tr>
    <tr>
      <td>SVELTE</td>
      <td>2.2</td>
      <td>2.5</td>
    </tr>
    <tr>
      <td>REACT (+DOM)</td>
      <td>2.09</td>
      <td>2.57</td>
    </tr>
    <tr>
      <td>GLIMMER</td>
      <td>2.81</td>
      <td>4.12</td>
    </tr>
    <tr>
      <td>ANGULAR</td>
      <td>3.2</td>
      <td>4.3</td>
    </tr>
  </tbody>
</table>

## References

- [Pinterest ❤️Mobile Web @jsday Italy // Speaker Deck](https://speakerdeck.com/zackargyle/pinterest-mobile-web-at-jsday-italy)
- [A Pinterest Progressive Web App Performance Case Study](https://medium.com/dev-channel/a-pinterest-progressive-web-app-performance-case-study-3bd6ed2e6154)
- [davidmarkclements/v8-perf: Exploring v8 performance characteristics in Node across v8 versions 5.1, 5.8, 5.9, 6.0 and 6.1](https://github.com/davidmarkclements/v8-perf)
- [Server Side Rendering from the trenches // Speaker Deck](https://speakerdeck.com/willdurand/server-side-rendering-from-the-trenches)
- [ESModulesNextRev.jsday.0518 - Google Slides](https://goo.gl/qRzeY3)
- [10KB or Bust: The Delicate Power of Webpack and Babel // Speaker Deck](https://speakerdeck.com/btholt/10kb-or-bust-the-delicate-power-of-webpack-and-babel)
- [Releases · babel/babel](https://github.com/babel/babel/releases)
- [We’re nearing the 7.0 Babel release. Here’s all the cool stuff we’ve been doing.](https://medium.freecodecamp.org/were-nearing-the-7-0-babel-release-here-s-all-the-cool-stuff-we-ve-been-doing-8c1ade684039)
- [jsDay 2018 schedule - Joind.in](https://joind.in/event/jsday-2018/schedule/list)
