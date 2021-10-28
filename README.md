# ytt-buildpack

Buildpack for rendering YTT templates in Cloud Foundry apps.

**Status:** Highly Experimental

## Usage

To push an app and render its ytt templates at staging time:
```
$ git clone https://github.com/Gerg/ytt-sample-app.git
$ cd ytt-sample-app
$ cf push ytt -b https://github.com/Gerg/ytt-buildpack.git -b ruby_buildpack --no-start
$ cf set-env ytt YTT_DATA "#@data/values\n---\nname: World"
$ cf set-env ytt YTT_OUTPUT config.yml
$ cf restage ytt
```

## Why This is a Bad Idea

It's of dubious value to render templates at staging time. Templates can easily
be rendered prior to uploading packages or at runtime.

This is also built on pretty shaky ground, because buildpacks are not supposed
to modify the `build` directory, which this one does with wild abandon:

> The supply script must not modify anything outside of the deps/index
> directory. Staging may fail if such modification is detected.
- Source: https://docs.cloudfoundry.org/buildpacks/understand-buildpacks.html
