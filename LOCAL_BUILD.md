# Building this branch on another Mac

This is `smartwatch-manager-review-pr-plus-local-changes`: the smartwatch
manager review branch plus local changes. It builds Loop with the Pebble plugin
(LupedSmartwatchKit) that serves both Luped CGM (watchface) and Luped Remote
(watchapp), with every remote action declared. Loop's own Watch Permissions
still decide each action; all are off until you turn them on.

## Clone

```sh
git clone --recurse-submodules -b smartwatch-manager-review-pr-plus-local-changes \
  https://github.com/dabear/LoopWorkspace.git LoopWorkspace
```

Every submodule commit is on GitHub. The local branches the submodules follow
(see `.gitmodules`):

| Submodule          | Repository                       | Branch                                            |
| ------------------ | -------------------------------- | ------------------------------------------------- |
| Loop               | bjorninge-no-personal/LoopPebble | `smartwatch-manager-review-pr-plus-local-changes` |
| LupedSmartwatchKit | dabear/LupedSmartwatchKit        | `smartwatch-manager-review-pr-plus-local-changes` |
| LoopKit            | dabear/LoopKit                   | `smartwatch-manager-review-pr-plus-local-changes` |

Swift packages resolve from `Package.resolved` (elbbepTransport 1.2.0).

## Signing

`LoopConfigOverride.xcconfig` sets the bundle ID (`no.bjorninge.2024v1.loop`)
and the feature flags. The team ID is not in git: it comes from
`../LoopConfigOverride.xcconfig`, one level above the clone. Create it there:

```
LOOP_DEVELOPMENT_TEAM = <your team ID>
```

Without it the build is unsigned and won't install on a phone.

## Build and install

Open `LoopWorkspace.xcworkspace`, pick the `LoopWorkspace` scheme and your
iPhone, and run. From the command line:

```sh
xcodebuild -workspace LoopWorkspace.xcworkspace -scheme LoopWorkspace \
  -configuration Debug -destination 'id=<iPhone UDID>' \
  -allowProvisioningUpdates build
xcrun devicectl device install app --device <iPhone UDID> \
  <DerivedData>/Build/Products/Debug-iphoneos/Loop.app
```

## Watch

Install Luped CGM and Luped Remote from the Pebble app store, or their PBWs
from https://github.com/dabear/LupedPebbleWatchFace/releases. In Loop, add the
Pebble under Settings → Devices; Luped Remote is off until you turn it on
(Settings → Pebble → Watch App), which also links to it in the Pebble app store.

