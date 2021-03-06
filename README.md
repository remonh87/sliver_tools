# sliver_tools

A set of useful sliver tools that are missing from the flutter framework.


Here is a taste what you can make using this package

![Demo](https://raw.githubusercontent.com/Kavantix/sliver_tools/master/gifs/demo2.gif)

The structure of this app:
```dart
class Section extends State {
  @override
  Widget build(BuildContext context) {
    return MultiSliver(
      pushPinnedChildren: true,
      children: <Widget>[
        SliverPersistentHeader(
          pinned: true,
          ...
        ),
        if (!infinite)
          SliverAnimatedPaintExtent(
            child: SliverList(...),
          )
        else
          SliverList(...),
      ],
    );
  }
}

class NewsPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CustomScrollView(
      slivers: <Widget>[
        Section(infinite: false),
        Section(infinite: true),
      ],
    );
  }
}
```

## [MultiSliver](https://github.com/Kavantix/sliver_tools/blob/master/lib/src/multi_sliver.dart)

The `MultiSliver` widget allows for grouping of multiple slivers together such that they can be returned as a single widget.
For instance when one wants to wrap a few slivers with some padding or an inherited widget.


### Example
```dart
class WidgetThatReturnsASliver extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MultiSliver(
      pushPinnedChildren: false, // defaults to false
      children: <Widget>[
        SliverPersistentHeader(...),
        SliverList(...),
      ],
    );
  }
}
```

The `pushPinnedChildren` parameter allows for achieving a 'sticky header' effect by simply using pinned `SliverPersistentHeader` widgets (or any custom sliver that paints beyond its layoutExtent).

## [SliverStack](https://github.com/Kavantix/sliver_tools/blob/master/lib/src/sliver_stack.dart)

The `SliverStack` widget allows for stacking of both slivers and box widgets.
This can be useful for adding some decoration to a sliver.
Which is what some of the other widgets in this package use to get their desired effects.

### Example
```dart
class WidgetThatReturnsASliver extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return SliverStack(
      insetOnOverlap: false, // defaults to false
      children: <Widget>[
        SliverPositioned.fill(
          child: Container(
            decoration: BoxDecoration(
              color: Colors.white,
              boxShadow: const <BoxShadow>[
                BoxShadow(
                  offset: Offset(0, 4),
                  blurRadius: 8,
                  color: Colors.black26,
                )
              ],
              borderRadius: BorderRadius.circular(8),
            ),
          ),
        )
        SliverList(...),
      ],
    );
  }
}
```

The `insetOnOverlap` handles whether the positioned children should be inset (made smaller) when the sliver has overlap from a previous sliver.

## [SliverClip](https://github.com/Kavantix/sliver_tools/blob/master/lib/src/sliver_clip.dart)

The `SliverClip` widget will add a clip around its child from the child's paintOrigin to its paintExtent.
This is very useful and most likely what you want when using a pinned SliverPersistentHeader as child of the stack.

### Example
```dart
class WidgetThatReturnsASliver extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return SliverClip(
      clipOverlap: true, // defaults to true
      child: SliverList(...),
    );
  }
}
```

The `clipOverlap` parameter allows for configuring whether any overlap with the previous child should be clipped.
This can be useful when one has a SliverPersitentHeader above a SliverList and does not want to give the header an opaque background but also prevent the list from drawing underneath the header.


## [SliverAnimatedPaintExtent](https://github.com/Kavantix/sliver_tools/blob/master/lib/src/sliver_animated_paint_extent.dart)

The `SliverAnimatedPaintExtent` widget allows for having a smooth transition when a sliver changes the space it will occupy inside the viewport.
For instance when using a SliverList with a button below it that loads the next few items.


### Example
```dart
class WidgetThatReturnsASliver extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return SliverAnimatedPaintExtent(
      duration: const Duration(milliseconds: 150),
      child: SliverList(...),
    );
  }
}
```

## [SliverAnimatedSwitcher](https://github.com/Kavantix/sliver_tools/blob/master/lib/src/sliver_animated_switcher.dart)

The `SliverAnimatedSwitcher` widget is simply a pre-configured `AnimatedSwitcher` widget.
If one needs more options than supplied by this widget a regular `AnimatedSwitcher` can be used by giving it the `defaultLayoutBuilder` and `defaultTransitionBuilder` of `SliverAnimatedSwitcher`.
