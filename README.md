# Pull-Down Button

[![Dart SDK Version](https://badgen.net/pub/sdk-version/pull_down_button)](https://pub.dev/packages/pull_down_button)
[![Pub Version](https://badgen.net/pub/v/pull_down_button?icon=flutter)](https://pub.dev/packages/pull_down_button)
[![style: very good analysis](https://img.shields.io/badge/style-very_good_analysis-B22C89.svg)](https://pub.dev/packages/very_good_analysis)

**pull_down_button** is a rework of Flutter's `PopupMenuButton` to be styled like
[Pop-Up](https://developer.apple.com/design/human-interface-guidelines/components/menus-and-actions/pop-up-buttons) and
[Pull-Down](https://developer.apple.com/design/human-interface-guidelines/components/menus-and-actions/pull-down-buttons)
Buttons from iOS 14+ with some additional customisation options.

---

This package only tries to visually replicate native counterpart, some parts might be somewhat different.

#### Flutter availability:

Since this package uses new Flutter feature `ThemeExtension` for theming, minimum supported version is stable **3.0.0**.

---

### Contents:

- [PullDownButton](#pulldownbutton)
  - [PullDownMenuItem](#pulldownmenuitem)
  - [SelectablePullDownMenuItem](#selectablepulldownmenuitem)
  - [PullDownMenuActionsRow](#pulldownmenuactionsrow)
  - [PullDownMenuDivider](#pulldownmenudivider)
  - [PullDownMenuTitle](#pulldownmenutitle)
- [Theming](#theming)
  - [PullDownButtonTheme](#pulldownbuttontheme)
- [Contributions](#contributions)

---

## PullDownButton

![PullDownButton example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/usage.png)

`PullDownButton` is a widget used to show pull-down menu. Unlike `PopupMenuButton`, `PullDownButton` allows better customization of button that will be used to show pull-down menu via `buttonBuilder` builder function.

While pull-down menu is opened, button from where this menu was called will have lower opacity.

```dart
PullDownButton(
  itemBuilder: (context) => [
    PullDownMenuItem(
      title: 'Menu item',
      onTap: () {},
    ),
    const PullDownMenuDivider(),
    PullDownMenuItem(
      title: 'Menu item 2',
      onTap: () {},
    ),
  ],
  position: PullDownMenuPosition.under,
  buttonBuilder: (context, showMenu) => CupertinoButton(
    onPressed: showMenu,
    padding: EdgeInsets.zero,
    child: const Icon(CupertinoIcons.ellipsis_circle),
  ),
);
```

<details><summary>Parameters table</summary>

| Parameters         | Description                                                                                              | Value                        |
| ------------------ | -------------------------------------------------------------------------------------------------------- | ---------------------------- |
| itemBuilder        | Called when the button is pressed to create the items to show in the menu.                               | required                     |
| buttonBuilder      | Builder that provides `BuildContext` as well as `showMenu` function to pass to any custom button widget. | required                     |
| onCanceled         | Called when the user dismisses the pull-down menu.                                                       | optional                     |
| offset             | The offset is applied relative to the initial position set by the `position`.                            | `Offset.zero`                |
| position           | Whether the popup menu is positioned over or under the popup menu button.                                | `PullDownMenuPosition.above` |
| backgroundColor    | The background color of pull-down menu.                                                                  | optional                     |
| widthConfiguration | Pull-down menu width.                                                                                    | optional                     |
| applyOpacity       | Whether to apply opacity on `buttonBuilder` when menu is open.                                           | optional                     |

`backgroundColor` usually has opacity in range of **0.7-0.8** so that menu has blur effect.

</details>

#### PullDownMenuPosition

The way `PullDownButton` positions its pull-down menu.

Available options:

- `over` - menu is positioned over an anchor. Will attempt to fill as much space as possible;
- `under` - menu is positioned under an anchor and is forced to be under an anchor;
- `above` - menu is positioned above an anchor and is forced to always be above an anchor;
- `automatic` - menu is positioned under or above an anchor depending on side that has more space available.

---

### PullDownMenuItem

![PullDownMenuItem example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/item.png)

`PullDownMenuItem` is a widget used to create cupertino style pull-down menu item.

```dart
PullDownMenuItem(
  title: 'Add to favourites',
  onTap: () {},
  icon: CupertinoIcons.star,
),
```

<details><summary>Parameters table</summary>

| Parameters       | Description                                      | Value    |
| ---------------- | ------------------------------------------------ | -------- |
| onTap            | Called when the menu item is tapped.             | required |
| enabled          | Whether the user is permitted to tap this item.  | true     |
| title            | Title of this `PullDownMenuItem`.                | required |
| icon             | Trailing icon of this `PullDownMenuItem`.        | optional |
| iconColor        | Trailing icon's color.                           | optional |
| iconSize         | Size of trailing icon.                           | optional |
| iconWidget       | Custom trailing widget.                          | optional |
| textStyle        | Title text style.                                | optional |
| isDestructive    | Whether this item represents destructive action. | false    |
| destructiveColor | Color for destructive action.                    | optional |
| onHoverColor     | On hover color.                                  | optional |
| onHoverTextStyle | On hover text style.                             | optional |

 </details>

---

### SelectablePullDownMenuItem

![SelectablePullDownMenuItem example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/selectable_item.png)

`SelectablePullDownMenuItem` is a widget used to create cupertino style pull-down menu item with selection state.

```dart
SelectablePullDownMenuItem(
  title: 'Order by size',
  selected: true,
  onTap: () {},
  icon: CupertinoIcons.chevron_down,
),
```

##### Note:

Based on [guidelines](https://developer.apple.com/design/human-interface-guidelines/components/menus-and-actions/pull-down-buttons), if menu items contains at least one tappable menu item of type `SelectablePullDownMenuItem` all of `PullDownMenuItem`s should also be of type `SelectablePullDownMenuItem` (to insert additional padding so all items have same). Although, manual change of all `PullDownMenuItem`s is not needed, it is done automatically.

<details><summary>Parameters table</summary>

`SelectablePullDownMenuItem` uses all of `PullDownMenuItem` parameters as well as a few `SelectablePullDownMenuItem` specific:

| Parameters      | Description                                           | Value    |
| --------------- | ----------------------------------------------------- | -------- |
| selected        | Whether to display a checkmark next to the menu item. | false    |
| checkmark       | Checkmark icon.                                       | optional |
| checkmarkWeight | Weight of checkmark icon.                             | optional |
| checkmarkSize   | Size of checkmark icon                                | optional |

</details>

---

### PullDownMenuActionsRow

![PullDownMenuActionsRow example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/actions_row.png)

`PullDownMenuActionsRow` is a widget used to create cupertino style pull-down menu row of actions
([small or medium size](https://developer.apple.com/documentation/uikit/uimenu/4013313-preferredelementsize)).

```dart
PullDownMenuActionsRow.medium(
  items: [
    PullDownMenuIconAction(
      enabled: false,
      onTap: () {},
      title: 'Inbox',
      icon: CupertinoIcons.tray_arrow_down,
    ),
    PullDownMenuIconAction(
      onTap: () {},
      title: 'Archive',
      icon: CupertinoIcons.archivebox,
    ),
    PullDownMenuIconAction(
      onTap: () {},
      title: 'Trash',
      isDestructive: true,
      icon: CupertinoIcons.delete,
    ),
  ],
),
```

| Parameters   | Description                                      | Value    |
| ------------ | ------------------------------------------------ | -------- |
| items        | List of `PullDownMenuIconAction`.                | required |
| dividerColor | Color of vertical dividers used to split `item`. | optional |

### PullDownMenuIconAction

`PullDownMenuIconAction` is used to populate `PullDownMenuActionsRow.items`.
Depending on `PullDownMenuActionsRow` size might be either icon only or icon and title in vertical array

<details><summary>Parameters table</summary>

`PullDownMenuIconAction` uses all of `PullDownMenuItem` parameters except `textStyle`. Instead `PullDownMenuIconAction` uses `iconActionTextStyle`:

| Parameters          | Description                                                                         | Value    |
| ------------------- | ----------------------------------------------------------------------------------- | -------- |
| iconActionTextStyle | Text style for `PullDownMenuIconAction` when using `PullDownMenuActionsRow.medium`. | optional |

</details>

---

### PullDownMenuDivider

![PullDownMenuDivider example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/dividers.png)

`PullDownMenuDivider` is a widget used to create cupertino style pull-down menu divider (small or large).

```dart
const PullDownMenuDivider(),
```

or to create large divider:

```dart
const PullDownMenuDivider.large(),
```

There is also convenience method to wrap multiple menu items with small dividers:

```dart
...PullDownMenuDivider.wrapWithDivider([
  PullDownMenuItem(
    title: 'Menu item',
    onTap: () {},
  ),
  PullDownMenuItem(
    title: 'Menu item 2',
    onTap: () {},
  ),
]),
```

---

### PullDownMenuTitle

![PullDownMenuTitle example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/title.png)

`PullDownMenuTitle` is a widget used to create cupertino style pull-down menu title (usually at the top of menu).

```dart
const PullDownMenuTitle(title: Text('Pull-down menu')),
```

| Parameters | Description              | Value    |
| ---------- | ------------------------ | -------- |
| title      | Title widget.            | required |
| titleStyle | Title widget text style. | optional |

---

## Theming

This package also provides additional customisation. By default, iOS15 theme is used, but it is also possible to override defaults with widget parameters (see above) or with `PullDownButtonTheme` theme extension.

#### Default theme

| Light Theme                                                                                                               | Dark Theme                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| ![light default theme example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/usage.png) | ![dark default theme example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/usage_dark.png) |

### PullDownButtonTheme

To use `PullDownButtonTheme` define it in your `ThemeData` as follows:

```dart
ThemeData(
  ...,
  extensions: [
    PullDownButtonTheme(
      backgroundColor: Colors.grey,
      iconSize: 24,
      dividerColor: Colors.black,
    ),
  ],
),
```

<details><summary>Parameters table (all values are optional)</summary>

| Parameters          | Description                                                                   |
| ------------------- | ----------------------------------------------------------------------------- |
| backgroundColor     | The background color of pull-down menu.                                       |
| dividerColor        | Small divider color.                                                          |
| largeDividerColor   | Large divider color.                                                          |
| destructiveColor    | Color for destructive action.                                                 |
| iconSize            | Size of trailing icon.                                                        |
| checkmark           | Checkmark icon.                                                               |
| checkmarkWeight     | Weight of checkmark icon.                                                     |
| checkmarkSize       | Size of checkmark icon                                                        |
| textStyle           | `PullDownMenuItem` text style.                                                |
| iconActionTextStyle | `PullDownMenuIconAction` text style.                                          |
| titleStyle          | `PullDownMenuTitle` style.                                                    |
| widthConfiguration  | Pull-down menu width.                                                         |
| applyOpacity        | Whether to apply opacity on `PullDownButton.buttonBuilder` when menu is open. |
| onHoverColor        | On hover color of `PullDownMenuItem`                                          |
| onHoverTextStyle    | On hover text style of `PullDownMenuItem`                                     |

`backgroundColor` usually has opacity in range of **0.7-0.8** so that menu has blur effect.
`largeDividerColor` is usually lighter than `dividerColor`.

</details>

#### PullDownButtonInheritedTheme

If defining `PullDownButtonTheme` in `ThemeData` is not possible, for example if you are using `CupertinoApp`, you can use `PullDownButtonInheritedTheme`:

```dart
CupertinoApp(
  builder: (context, child) => PullDownButtonInheritedTheme(
    data: const PullDownButtonTheme(
      ...
    ),
    child: child!,
  ),
  home: ...,
)
```

---

Here is example of using `PullDownButtonTheme` with Material 3 color scheme colors
(generated from `CupertinoColors.systemBlue` with `ColorScheme.fromSeed`) from [Material 3 Menu specs](https://m3.material.io/components/menus/specs).

| Custom Material 3 light theme                                                                                           | Custom Material 3 dark theme                                                                                          |
| ----------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| ![light theme example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/theme_light.png) | ![dark theme example](https://raw.githubusercontent.com/notDmDrl/pull_down_button/main/readme_content/theme_dark.png) |

---

### Contributions

Feel free to contribute to this project.

Please file feature requests and bugs at the [issue tracker](https://github.com/notDmDrl/pull_down_button).

If you fixed a bug or implemented a feature by yourself, feel free to send a [pull request](https://github.com/notDmDrl/pull_down_button/pulls).
