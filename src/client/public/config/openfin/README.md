# OpenFin manifests for RT

These files contain the manifests for the different flavours of the OpenFin version of 
Reactive Trader. They are used in two ways: 

1. Passing them to the `openfin` cli during development
    - E.g. by doing `npm run openfin`, which in turn runs `openfin -c public/config/openfin/local.json -l`
2. To generate the installers. See [`src/client/install/README.md`](../../../install/README.md) for a detailed guide. 

Note that [the installers](https://openfin.co/documentation/options/#installer) are just a 
wrapper containing **the URL** to the manifest, (**not** the manifest file itself), so there's 
no need to re-generate an installer when making changes to the manifests.

# OpenFin Layouts V2

Refs:
- 
- New 'Platform' API guide: [https://developers.openfin.co/docs/platform-api].<br>
  Use in code e.g. `fin.Platform.getCurrentSync().getSnapshot().then(snap => console.log(snap))`
- Platform API docs / tutorials (***note the 'canary' URL***) e.g. [https://cdn.openfin.co/docs/javascript/canary/tutorial-Platform.applySnapshot.html]
- Layout generator: [https://openfin.github.io/golden-prototype/config-gen]
- The [latest stable](https://developer.openfin.co/versions/?product=Runtime&version=14.78.46.23) runtime version announced Layouts V2, but the refs there already appear to be out of date - I have used the latest alpha/beta (14.78.47.23), which is recommended by the Platform API docs.

Notes:
- 
- new 'Platform'-friendly manifests have "platform" and "snapshot" sub-sections, but still require a non-null "startup_app" otherwise Mac OF Cli will barf (due to reportUsage function, which is a bit dubious)
- layout content is of type 'row', 'column', 'stack' (tabbed view) or 'component' (a leaf item, like the analytics, blotter or an FX tile ...
- using a 'component' type as a direct child of a row or column works, but the effective snapshot will have 'stack' types wrapped around any leaf components
- layout generator page is quite useful for playing with the design, although I just wrote it by hand in the end - https://openfin.github.io/golden-prototype/config-gen - it was slightly useful as a way of quickly discovering snapshot layout properties

Issues:
-
- [OpenFin] cannot hide 'stack' tab headers at present
- [OpenFin] row/column layout will not support current responsive FX Tile layout
- [OpenFin] row/column layout does not allow scrolled view e.g. FX Tiles should be a minimum size and scrollbar will take care of excess real estate
- [OpenFin] no way to set a fixed size e.g. for analytics side bar
- [OpenFin] resizing / rearranging is currently a bad experience (ALL views content becomes white during operation, so no visual feedback of resized content until after the new size is applied)
- [OpenFin] tried to suppress close feature for analytics view - isClosable attributes seen in config generator at stack level, but setting to false appears to have no effect
- [OpenFin] title property for 'component' has no effect when a URL is given (which is always), as it is overridden by content page title - this is not really an issue for RTC as we do not want a tab header anyway
- [OpenFin] on startup from a manifest with a snapshot definition, or when a view is dragged out of a window, dragging the window header is unresponsive - I had to resize the window and then I was able to drag by the window header
- [OpenFin] when a tab (component in a stack view) is dragged out of the window, it is hard to find a droparea in a busy screen, as it is accepted by a number of applications like Chrome, Mac Finder etc.
- [OpenFin] when a view that has been dragged out of a window, and now occupies its own separate window, is dragged back into the original window, the only drop targets available are the tab headers (i.e. it MUST join a tab group (stack) and cannot be made a docked view (new stack view)


- [Adaptive] embedding the FX tiles, blotter, analytics by URL in the layout means that the views do not have the popout button but they do have the close (or restore) button, which does nothing i.e. they are in 'popped out' mode
- [Adaptive] how do we embed the FX tile 'currency filter' / 'normal to line chart toggle' header bar - should this be accessible via a separate URL, like blotter, analytics, individual FX tiles



