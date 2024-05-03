# Auto Complete Component

Demo of a Vue component for auto complete search. Styles built with SCSS. Loading animation found online.

[link to demo](https://ianawilliams.github.io/auto-complete/)

| Props      | Type | Default | Description | 
| ----------- | ----------- | ----------- | ----------- |
| clearable | Boolean | true | if set to true a clear button will show in right side of search bar when text has been entered |
| place-holder | String | "Search..." | Message that appears when search bar is empty |
| absolute | Boolean | false | Makes the dropdown absolute instead of pushing content down |
| options | String Array | [] | List of items to show in dropdown |
| max-height | String | none | A css string that will be used for max height of dropdown |
| loading | Boolean | false | Activates loading icon on dropdown |
| suggest-all | Boolean | false | If true, all options will show when search bar is empty |
| disable-filter | Boolean | false | Disables filtering from a set list. Used when filtering will be done by an API call from parent |
| over-ride-results | String Array | [] |  Resuls from API will be put here |

Debounce implemented with Lodash
