---
title: '今週のプロジェクト: Kap'
author:
  - skllcrn
  - sindresorhus
  - zeke
date: '2017-01-31'
---

Electron のコミュニティは急速に成長しており、協力な新しいアプリやツールが驚異的速さで開発されています。 この創造的な勢いを祝い、この新しいプロジェクトらをいくつかコミュニティにお知らせするため、注目の Electron 関連プロジェクトを取り上げた週刊ブログシリーズを始めることにしました。

---

このシリーズの第一回目の記事では、フリーランスのデザイナーと開発者で構成される世界中に分かれたチーム [Wulkano](https://wulkano.com/) が開発したオープンソースの画面録画アプリ [Kap](https://getkap.co/) を特集します。

[![Kap Screencast](https://cloud.githubusercontent.com/assets/2289/22439463/8f1e509e-e6e4-11e6-9c32-3a9db63fc9a1.gif)](https://getkap.co/)

## Kap とは何ですか?

[Kap はオープンソースの画面録画機](https://getkap.co) です。主にデザイナーや開発者が自分の作品を簡単にキャプチャするために作りました。 プロトタイプのアニメーションを共有したり、バグを文書化したり、ネタ GIF を作成したりといったことに使われています。

老若男女の方々が、教育現場、スクリーンキャスト、チュートリアル などで使用しているようです。 製品のアセット作成にも使われています! この小さなサイドプロジェクトがこれだけ受け入れられていることに、ただただ圧倒されるばかりです。

## どうしてこのアプリを構築したのですか?

非常に良い質問ですね。画面録画機が無いからといったわけではありません! 私たちは、画面録画機の代替物が複雑すぎたり、高価すぎたり、限定的すぎたりしていると感じました。 常用としては、*ちょうどいい* と感じるものが何もなかったのです。 また、仕事に使うツールはオープンソースであったほうが素晴らしいとも思います。 [Kap の構築は、特に何もしなかったようなものです](https://medium.com/wulkano-friends/from-idea-to-product-and-beyond-a12850403c38)。 自分たちが使いたいと思うツールのイメージになるように、小さな改良を積み重ねてこだわりました。

However, and maybe most importantly, Kap has become a place for us to leave our worries at the door and just have fun building something for ourselves and people like us. It's so important to create an environment where you get to just vent, try new thins and enjoy your craft. No requirements, no pressure, no expectations. Should designers and developers side project? Why, yes. Yes, they should.

## Why did you choose to build Kap on Electron?

There were a number of reasons:

* Web tech
* Most of the team are web developers
* We're invested in JavaScript
* It opens the door for more people to contribute
* Electron itself is open-source
* The power and easily maintainable modularity of `node_modules`
* Cross-platform possibilities

We think the future of apps are in the browser, but we're not quite there yet. Electron is an important step in the journey towards that future. It not only makes the apps themselves more accessible, but also the code they're built with. An interesting thought is imagining a future where the OS is a browser, and the tabs are essentially Electron apps.

Additionally, being primarily web developers, we're big fans of the isomorphic nature of JavaScript, in that you can run JS on the client, server, and now the desktop. With web tech (HTML, CSS and JS), many things are much simpler than native: Faster prototyping, less code, flexbox > auto-layout (macOS/iOS).

## What are some challenges you've faced while building Kap?

Using the resources Electron has available to record the screen was the biggest challenge. They simply weren't performant enough to meet our requirements and would render the project a failure in our eyes. Though at no fault of Electron itself, there's still a gap between native development and building desktop apps with web tech.

We spent a lot of time trying to work around the poor performance of the `getUserMedia` API, an issue originating in Chromium. One of our main goals when we set out to make Kap was to build the entire app with web tech. After trying everything we could to get it working (the minimum requirement being 30 FPS on a Retina screen), we eventually had to find another solution.

## I see some Swift code in the repo. What's that about?

Being forced to look for alternatives to `getUserMedia`, we started experimenting with `ffmpeg`. Besides being one of the best tools for audio and video conversion it has the functionality of recording the screen in almost any OS, and we were able to record crispy video meeting our minimum requirement of 30 FPS on a Retina screen. 問題? The performance was ":weary:", the CPU usage was going haywire. So we went back to the drawing board, discussed our options and realised that we had to make a compromise. That resulted in [Aperture](https://github.com/wulkano/aperture), our own screen recording library for macOS written in Swift.

## Electron はどういった領域で改善されるべきでしょうか?

We all know that Electron apps can have a thing for using RAM, but again, that's really a Chromium thing. It's part of how it works and it really depends on what you're running, for example Kap and Hyper typically use less than 100MB of memory.

One of the biggest areas of improvement that we see is payload, particularly how Electron distributes Chromium. One idea would be to have a shared Electron core and make app installers check if it's already present on the system.

Creating cross-platform Electron apps could be a better experience. Right now there are too many inconsistencies, platform-specific APIs, and missing features between platforms, making your codebase littered with if-else statements. For example, vibrancy is only supported on macOS, the auto-updater works differently on macOS and Windows, and is not even supported on Linux. Transparency is a hit or miss on Linux, usually miss.

It should also be easier to call native system APIs. Electron comes with a very good set of APIs, but sometimes you need functionality it doesn't provide. Creating a native Node.js addon is an option, but it's painful to work with. Ideally Electron would ship with a good [FFI](https://en.wikipedia.org/wiki/Foreign_function_interface) API, like [`fastcall`](https://github.com/cmake-js/fastcall). This would have enabled us to write the Swift part in JavaScript instead.

## Electron の好きなところは何ですか?

Our favorite thing is easily the fact that anyone with knowledge of creating for the web can build and contribute to multi-platform native experiences. Not to mention the ease and joy of developing on it, the excellent documentation and the thriving ecosystem.

From a front-end perspective, building Kap felt no different than building a simple website using browser APIs. Electron does a really great job of making app development similar (basically identical) to web development. So simple in fact that there was no need for frameworks or similar to help us, just clean and modular JS and CSS.

We are also huge fans of the team building it, their dedication and support, and the active and friendly community they maintain. Hugs to all of you!

## What's coming next in Kap?

The next step for us is to review the app in preparation for our 2.0.0 milestone, which includes a React re-write in addition to support for plugins, allowing developers to extend the functionality of Kap! We invite everyone to follow to project and contribute on our [GitHub repository](https://github.com/wulkano/kap). We're listening and want to hear from as many of you as possible, [let us know how we can make Kap the best possible tool it can be for you](https://wulkano.typeform.com/to/BIvJKz)!

## What is Wulkano?

[Wulkano](https://wulkano.com) is a design studio and digital collective, a team of remote technologists who love working together on both client gigs and our own projects. We're a distributed but tight knit group of people from different places and backgrounds, sharing knowledge, ideas, experiences, but most importantly silly GIFs and memes, in our virtual office (which happens to be the Electron based Slack!).

## 他の開発者に役立つ Electron のノウハウはありますか?

Take advantage of and get involved in the fantastic [community](https://discuss.atom.io/c/electron), check out [Awesome Electron](https://github.com/sindresorhus/awesome-electron), look at [examples](https://github.com/electron/electron-api-demos) and make use of the great [docs](https://electronjs.org/docs/)!

