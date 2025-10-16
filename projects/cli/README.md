# Elm Land

[![Discord](https://badgen.net/badge/icon/discord?icon=discord&label&color=7289da)](https://join.elm.land) [![Twitter](https://badgen.net/badge/icon/twitter?icon=twitter&label&color=00acee)](https://twitter.com/elmland_) [![GitHub](https://badgen.net/badge/icon/github?icon=github&label&color=4078c0)](https://www.github.com/elm-land/elm-land)

Elm Land – A fork including `withOnSharedMsg`

## 🌱 About this fork

This is a friendly, thankful and respectful fork of [Elm Land](https://github.com/elm-land/elm-land), solving one specific problem: **enabling pages and layouts to react to shared messages**.

### The problem

Elm Land already gives pages access to the latest shared **state** (via `Shared.Model`), but there's no way for pages to react to **messages** that happen in the shared layer - like when a WebSocket message arrives, when a subscription fires, or when any event occurs that updates the shared state. Pages can respond to URL changes using `Page.withOnUrlChanged`, but not to things that _happen_ in the shared layer at a point in time.

### The solution

This fork adds `Page.withOnSharedMsg` and `Layout.withOnSharedMsg`, following the same pattern as existing Elm Land APIs:

```elm
page : Shared.Model -> Route () -> Page Model Msg
page shared route =
    Page.new
        { init = init
        , update = update
        , view = view
        , subscriptions = subscriptions
        }
        |> Page.withOnSharedMsg
            (\sharedMsg ->
                case sharedMsg of
                    Shared.Msg.ItemsSaved ->
                        ShowSuccessToast

                    _ ->
                        NoOp
            )
```

This allows pages and layouts to "subscribe" to shared state changes, making reactive UI updates natural and eliminating the need for manual synchronization workarounds.

There's a [PR](https://github.com/elm-land/elm-land/pull/205) open on this, but according to Ryan it will probably not be merged until Elm Land is out of beta (and when it is, his own solution will be more thorough).

**Only use this fork if you need the functionality described above!**

---

### Alpha release 🌱

Although Elm Land is still a work-in-progress, please feel free to tinker around until the big `v1.0.0` release!

If you're excited to try things out– come join the [Elm Land Discord](https://join.elm.land) to get help or share your experience!

## Using the CLI

The `elm-land` CLI comes with everything you need to create your next web application:

```
$ elm-land

🌈  Welcome to Elm Land! (v0.20.1)
    ⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺⎺
    Commonly used commands:

     elm-land init <folder-name> ...... create a new project
     elm-land server ................ run a local dev server
     elm-land build .......... build your app for production

    Other helpful commands:

     elm-land generate ............. generate Elm Land files
     elm-land add page <url> ................ add a new page
     elm-land add layout <name> ........... add a new layout
     elm-land customize <name> .. customize a default module
     elm-land routes ........... list all routes in your app

    Want to learn more? Visit https://elm.land/guide

```

## The source code

If you would like to see how it works, all the code is available and [open-source on GitHub](https://github.com/elm-land/elm-land).

The CLI, docs website, and all the other Elm Land projects can all be found in that single GitHub repo.

### Running the tests

The tests in this project are designed to verify that the [official guide](https://elm.land/guide) and [all of the examples](https://github.com/elm-land/elm-land/tree/main/examples) are accurate for users.

For that reason, we are using [bats](https://github.com/bats-core/bats-core) to make sure our CLI behaves as expected!

```bash
# Make sure you are in the `./cli` folder!
npm install
npm link
npm run setup
npm run test
```
