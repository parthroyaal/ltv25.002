# charting_library_wiki
/Widget-Constructor


You may specify Charting library widget parameters when calling its constructor. Here is an example of a call.

```javascript
new TradingView.widget({
    symbol: 'A',
    interval: 'D',
    timezone: "America/New_York",
    container_id: "tv_chart_container",
    locale: "ru",
    datafeed: new Datafeeds.UDFCompatibleDatafeed("https://demo_feed.tradingview.com")
});
```

Below is a complete list of supported parameters. Note that you can't change the parameters once the Charting Library is created. Use [Widget methods](Widget-Methods) if you wish to modify the parameters after the creation of the Charting Library.

### symbol, interval

The default symbol & time interval of your chart. The `interval` value is described in the [Section](Resolution). *Mandatory*

### timeframe

Sets the default timeframe of the chart. Timeframe is a period of bars that will be loaded and shown on the screen.
Valid timeframe is a number with a letter D for days and M for months.

### container_id

`id` is an attribute of a DOM element that you wish to include in the widget. *Mandatory*

### datafeed

JavaScript object that implements the ([JS API](JS-Api)) interface to supply the chart with data. *Mandatory*

### timezone

Default timezone of the chart. The time on the timescale is displayed according to this timezone.
See the [list of supported timezones](Symbology#timezone) for available values. Set it to `exchange` to use the exchange timezone. Use the [overrides](#overrides) section if you wish to override the default value.

### debug

Setting this property to `true` will make the chart write detailed API logs into the browser console. Alternatively, you can use the `charting_library_debug_mode` featureset to enable it.

### library_path

A path to a `static` folder.

### width, height

The desired size of a widget. Please make sure that there is enough space for the widget to be displayed correctly.

**Remark**: If you want the chart to use all the available space use the `fullscreen` parameter instead of setting it to '100%'.

### fullscreen

*Default:* `false`

Boolean value showing whether the chart should use all the available space in the window.

### autosize

*Default:* `false`

Boolean value showing whether the chart should use all the available space in the container and resize when the container itself is resized.

### symbol_search_request_delay

A threshold delay in milliseconds that is used to reduce the number of symbol search requests when the user is typing a name of a symbol in the search box.

### auto_save_delay

A threshold delay in seconds that is used to reduce the number of `onAutoSaveNeeded` calls.

### toolbar_bg

Background color of the toolbars.

### study_count_limit

*Starting from version 1.5.*

Maximum amount of studies on the chart of a multichart layout. Minimum value is 2.

### studies_access

*Starting from version 1.1.*

An object with the following structure:

```javascript
{
    type: "black" | "white",
    tools: [
        {
            name: "<study name>",
            grayed: true
        },
        < ... >
    ]
}
```

* `type` is the list type. Supported values are: `black` (all listed items should be disabled), `white` (only the listed items should be enabled).
* `tools` is an array of objects. Each object could have the following properties:
  * `name` (*Mandatory*) is the name of a study. Use the same names as in the pop-ups of indicators.
  * `grayed` is a boolean showing whether this study should be visible but look as if it's disabled. If the study is grayed out and user clicks it, then the `onGrayedObjectClicked` function is called.

### drawings_access

*Starting from version 1.1.*

This property has the same structure as the `studies_access`argument that is described above. Use the same names as you see in the UI.

**Remark**: There is a special case for font-based drawings. Use the "Font Icons" name for them. Those drawings cannot be enabled or disabled separately - the entire group will have to be either enabled or disabled.

### saved_data

JS object containing saved chart content. Use this parameter when creating the widget if you have a saved chart already. If you want to load the chart content when the chart is initialized then use [load() method](https://github.com/tradingview/charting_library/wiki/Widget-Methods#loadstate) of the widget.

### locale

Locale to be used by Charting Library. See [Localization](Localization) section for details.

### numeric_formatting

The object containing formatting options for numbers. The only possible option is `decimal_sign` currently.
Example: `numeric_formatting: { decimal_sign: "," }`

### customFormatters

It is an object that contains the following fields:

1. timeFormatter
1. dateFormatter

You can use these formatters to adjust the display format of the date and time values.
Both values are objects that include functions such as `format` and `formatLocal`:

```javascript
function format(date)
function formatLocal(date)
```

These functions should return the text that specifies date or time. `formatLocal` should convert date and time to local timezone.

Example:

```javascript
customFormatters: {
  timeFormatter: {
    format: function(date) { var _format_str = '%h:%m'; return _format_str.replace('%h', date.getUTCHours(), 2). replace('%m', date.getUTCMinutes(), 2). replace('%s', date.getUTCSeconds(), 2); }
  },
  dateFormatter: {
    format: function(date) { return date.getUTCFullYear() + '/' + date.getUTCMonth() + '/' + date.getUTCDate(); }
  }
}
```

### overrides

The object that contains new values for default widget properties.
You can override most of the Charting Library properties (which also may be edited by user through UI) using `overrides` parameter of Widget constructor. `overrides` is supposed to be an object. The keys of this object are the names of overridden properties. The values of these keys are the new values of the properties. Example:

```javascript
overrides: {
    "symbolWatermarkProperties.color": "rgba(0, 0, 0, 0)"
}
```

This code will make the watermark 100% opaque (invisible). All customizable properties are listed in [separate article](Overrides). You can use [Drawings-Overrides](Drawings-Overrides) starting from v 1.5.

### disabled_features, enabled_features

The array containing names of features that should be enabled/disabled by default. `Feature` means part of the functionality of the chart (part of the UI/UX). Supported features are listed [here](Featuresets).

Example:

```javascript
var widget = new TradingView.widget({
    /* .... */
    disabled_features: ["header_widget", "left_toolbar"],
    enabled_features: ["move_logo_to_main_pane"]
});
```

### snapshot_url

This URL is used to send a POST request with base64-encoded chart snapshots when a user presses the snapshot button. This endpoint should return the full URL of the saved image in the the response.

### indicators_file_name

Path to the file that contains your compiled indicators. See more details [here](Creating-Custom-Studies).

### preset

`preset` is a name of predefined widget settings. For now, the only value supported in the `preset` is  `mobile`. The example of this `preset` is [available here](https://demo_chart.tradingview.com/mobile_black.html).

### studies_overrides

Use this option to customize the style or inputs of the indicators. You can also customize the styles and inputs of the `Compare` series using this argument. See more details [here](Studies-Overrides)

### time_frames

List of visible timeframes that can be selected at the bottom of the chart.

Example:

```javascript
time_frames: [
    { text: "50y", resolution: "6M", description: "50 Years" },
    { text: "3y", resolution: "W", description: "3 Years", title: "3yr" },
    { text: "8m", resolution: "D", description: "8 Month" },
    { text: "3d", resolution: "5", description: "3 Days" },
    { text: "1000y", resolution: "W", description: "All", title: "All" },
]
```

Timeframe is an object containing the `text` and `resolution` properties. The `text` property should have the following format: `<integer><y|m|d>` ( \d+(y|m|d) as Regex ). Resolution is a string and its format is described here - [here](Resolution). See [this topic](Time-Frames) to learn more about timeframes.

The `description` property was added in v 1.7 and is displayed in the pop-up menu. This parameter is optional. If it isn't specified then the `title` or `text` property is used as a description.

The `title` property was added in v 1.9 and its value will override the default title generated based on the `text` property. This parameter is optional.

### charts_storage_url, client_id, user_id

These arguments are related to the high-level API for saving/loading the charts. See more details [here](Saving-and-Loading-Charts).

### charts_storage_api_version

A version of your backend. Supported values are: `"1.0"` | `"1.1"`. Study Templates are supported starting from version `"1.1"`.

### load_last_chart

Set this parameter to `true` if you want the library to load the last saved chart for a user (you should implement [save/load](Saving-and-Loading-Charts) first to make it work).

### theme

*Starting from version 1.13.*

Adds custom theme color for the chart. Supported values are: `"Light"` | `"Dark"`.

### custom_css_url

*Starting from version 1.4.*

Adds your custom CSS to the chart. `url` should be an absolute or relative path to the `static` folder.

### loading_screen

*Starting from version 1.12.*

Customization of the loading spinner. Value is an object with the following possible keys:

* `backgroundColor`
* `foregroundColor`

Example:

```javascript
loading_screen: { backgroundColor: "#000000" }
```

### favorites

Items that should be marked as favorite by default. This option requires that the usage of localstorage is disabled (see [featuresets](Featuresets) to know more). The `favorites` property is supposed to be an object. The following properties are supported:

* **intervals**: an array of time intervals that are marked as favorite. Example: `["D", "2D"]`
* **chartTypes**: an array of chart types that are marked as favorite. The names of chart types are identical to chart's UI in the English version. Example: `["Area", "Candles"]`.

### save_load_adapter

*Starting from version 1.12.*

An object containing the save/load functions. If it is available it should have the following methods:

**Chart layouts:**

 1. `getAllCharts(): Promise<ChartMetaInfo[]>`

    A function to get all saved charts.

    `ChartMetaInfo` is an object with the following fields:
     * `id` - unique ID of the chart.
     * `name` - name of the chart.
     * `symbol` - symbol of the chart.
     * `resolution` - resolution of the chart.
     * `timestamp` - last modified date (number of milliseconds since midnight `01/01/1970` UTC) of the chart.

 1. `removeChart(chartId): Promise<void>`

     A function to remove a chart. `chartId` is a unique ID of the chart (see `getAllCharts` above).

 1. `saveChart(chartData: ChartData): Promise<ChartId>`

     A function to save a chart.

    `ChartData` is an object with the following fields:
     * `id` - unique ID of the chart (may be `undefined` if it wasn't saved before).
     * `name` - name of the chart.
     * `symbol` - symbol of the chart.
     * `resolution` - resolution of the chart.
     * `content` - content of the chart.

    `ChartId` - unique ID of the chart (string)

 1. `getChartContent(chartId): Promise<ChartContent>`

     A function to load the chart from the server.

    `ChartContent` is a string with the chart content (see `ChartData::content` field in `saveChart` function).

**Study Templates:**

 1. `getAllStudyTemplates(): Promise<StudyTemplateMetaInfo[]>`

     A function to get all saved study templates.

    `StudyTemplateMetaInfo` is an object with the following fields:
     * `name` - name of the study template.

 1. `removeStudyTemplate(studyTemplateInfo: StudyTemplateMetaInfo): Promise<void>`

     A function to remove a study template.

 1. `saveStudyTemplate(studyTemplateData: StudyTemplateData): Promise<void>`

     A function to save a study template.

    `StudyTemplateData` is an object with the following fields:
     * `name` - name of the study template.
     * `content` - content of the study template.

 1. `getStudyTemplateContent(studyTemplateInfo: StudyTemplateMetaInfo): Promise<StudyTemplateContent>`

     A function to load a study template from the server.

    `StudyTemplateContent` - content of the study template (string)

 If both `charts_storage_url` and `save_load_adapter` are available  then `save_load_adapter` will be used.

 **IMPORTANT:** All functions should return a `Promise` (or `Promise`-like objects).

### settings_adapter

*Starting from version 1.11.*

An object that contains set/remove functions. Use it to save chart settings to your preferred storage (including server-side). If it is available then it should have the following methods:

1. `initialSettings: Object`

    An object with the initial settings

1. `setValue(key: string, value: string): void`

    A function that is called to store key/value pair.

1. `removeValue(key: string): void`

    A function that is called to remove a key.

## Trading Terminal only

### widgetbar

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

The object that contains settings for the widget panel on the right side of the chart. Watchlist, news and details widgets on the right side of the chart can be enabled using the `widgetbar` field in Widget constructor:

```javascript
widgetbar: {
    details: true,
    watchlist: true,
    watchlist_settings: {
        default_symbols: ["NYSE:AA", "NYSE:AAL", "NASDAQ:AAPL"],
        readonly: false
    }
}
```

* `details` (*default:* `false`): Enables details widget in the widget panel on the right.
* `watchlist` (*default:* `false`): Enables watchlist widget in the widget panel on the right.
* `watchlist_settings.default_symbols` (*default:* `[]`): Sets the list of default symbols for watchlist.
* `watchlist_settings.readonly` (*default:* `false`): Enables read-only mode for the watchlist.

### rss_news_feed

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

Use this property to change the RSS feed for news. You can set a different RSS for each symbol type or use a single RSS for all symbols. The object should have the `default` property, other properties are optional. The names of the properties match the symbol types. Each property is an object (or an array of objects) with the following properties:

1. `url` - is a URL to be requested. It can contain tags in curly brackets that will be replaced by the terminal: `{SYMBOL}`, `{TYPE}`, `{EXCHANGE}`.
1. `name` - is a name of the feed to be displayed underneath the news.

Here is an example:

```javascript
{
    "default": [ {
        url: "https://articlefeeds.nasdaq.com/nasdaq/symbols?symbol={SYMBOL}",
        name: "NASDAQ"
      }, {
        url: "http://feeds.finance.yahoo.com/rss/2.0/headline?s={SYMBOL}&region=US&lang=en-US",
        name: "Yahoo Finance"
      } ]
}
```

Another example:

```javascript
{
    "default": {
        url: "https://articlefeeds.nasdaq.com/nasdaq/symbols?symbol={SYMBOL}",
        name: "NASDAQ"
    }
}
```

One more example:

```javascript
{
    "default": {
        url: "https://articlefeeds.nasdaq.com/nasdaq/symbols?symbol={SYMBOL}",
        name: "NASDAQ"
     },
    "stock": {
        url: "http://feeds.finance.yahoo.com/rss/2.0/headline?s={SYMBOL}&region=US&lang=en-US",
        name: "Yahoo Finance"
    }
}
```

### news_provider

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

An object that specifies the news provider. It may contain the following properties:

1. `is_news_generic` - if set to `true` then the title of the news widget will not include a symbol name (`Headlines` will be included only). Otherwise `for SYMBOL_NAME` will be added.
1. `get_news` - use this property to set your own news getter function. Both the `symbol` and `callback` will be passed to the function.

    The callback function should be called with an array of news objects that have the following structure:

    * `title` (required) - the title of news item.
    * `published` (required) - the time of news item in ms (UTC).
    * `source` (optional) - source of the news item title.
    * `shortDescription` (optional) - Short description of a news item that will be displayed under the title.
    * `link` (optional) - URL to the news story.
    * `fullDescription` (optional) - full description (body) of a news item.

    **NOTE:** When a user clicks on a news item a new tab with the `link` URL will be opened. If `link` is not specified then a pop-up dialog with `fullDescription` will be shown.

    **NOTE 2:** If both `news_provider` and `rss_news_feed` are available then the `rss_news_feed` will be ignored.

Example:

```javascript
news_provider: {
    is_news_generic: true,
    get_news: function(symbol, callback) {
        callback([
            {
                title: 'News for symbol ' + symbol,
                shortDescription: 'Short description of the news item',
                fullDescription: 'Full description of the news item',
                published: new Date().valueOf(),
                source: 'My own source of news',
                link: 'https://www.tradingview.com/'
            },
            {
                title: 'Another news for symbol ' + symbol,
                shortDescription: 'Short description of the news item',
                fullDescription: 'Full description of the news item. Long text here.',
                published: new Date().valueOf(),
                source: 'My own source of news',
            }
        ]);
    }
}
```

### brokerFactory

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

Use this field to pass the function that returns a new object which implements [Broker API](Broker-API). This is a function that accepts [Trading Host](Trading-Host) and returns [Broker API](Broker-API).

### brokerConfig

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

Use this field to set the configuration flags for the Trading Terminal. [Read more](Trading-Objects-and-Constants#configflags-object).

## See Also

* [Customization Overview](Customization-Overview)
* [Widget Methods](Widget-Methods)
* [Featuresets](Featuresets)
* [Saving and Loading Charts](Saving-and-Loading-Charts)
* [Overriding Default Properties of the Studies](Studies-Overrides)
* [Overriding Default Properties of the Chart](Overrides)



# charting_library_wiki
/Widget-Methods

Below is the list of methods that the widget supports. You can call them using the widget object that is returned to you by the widget's constructor.

**Remark**: Please note that it's safe to call any method only **after** onChartReady callback function is called.

Example:

```javascript
widget.onChartReady(function() {
    // It's now safe to call any other methods of the widget
});
```

## Methods

* [Subscribing To Chart Events](#subscribing-to-chart-events)
  * [onChartReady(callback)](#onchartreadycallback)
  * [headerReady()](#headerready)
  * [onGrayedObjectClicked(callback)](#ongrayedobjectclickedcallback)
  * [onShortcut(shortcut, callback)](#onshortcutshortcut-callback)
  * [subscribe(event, callback)](#subscribeevent-callback)
  * [unsubscribe(event, callback)](#unsubscribeevent-callback)
* [Chart Actions](#chart-actions)
  * [chart()](#chart)
  * [setLanguage(locale)](#setlanguagelocale)
  * [setSymbol(symbol, interval, callback)](#setsymbolsymbol-interval-callback)
  * [remove()](#remove)
  * [closePopupsAndDialogs()](#closepopupsanddialogs)
  * [selectLineTool(drawingId)](#selectlinetooldrawingid)
  * [selectedLineTool()](#selectedlinetool)
  * [takeScreenshot()](#takescreenshot)
  * [lockAllDrawingTools](#lockalldrawingtools)
  * [hideAllDrawingTools](#hidealldrawingtools)
* [Saving/Loading Charts](#savingloading-charts)
  * [save(callback)](#savecallback)
  * [load(state)](#loadstate)
  * [getSavedCharts(callback)](#getsavedchartscallback)
  * [loadChartFromServer(chartRecord)](#loadchartfromserverchartrecord)
  * [saveChartToServer(onCompleteCallback, onFailCallback, saveAsSnapshot, options)](#savecharttoserveroncompletecallback-onfailcallback-saveassnapshot-options)
  * [removeChartFromServer(chartId, onCompleteCallback)](#removechartfromserverchartid-oncompletecallback)
* [Custom UI Controls](#custom-ui-controls)
  * [onContextMenu(callback)](#oncontextmenucallback)
  * [createButton(options)](#createbuttonoptions)
* [Dialogs](#dialogs)
  * [showNoticeDialog(params)](#shownoticedialogparams)
  * [showConfirmDialog(params)](#showconfirmdialogparams)
  * [showLoadChartDialog()](#showloadchartdialog)
  * [showSaveAsChartDialog()](#showsaveaschartdialog)
* [Getters](#getters)
  * [symbolInterval(callback)](#symbolinterval)
  * [mainSeriesPriceFormatter()](#mainseriespriceformatter)
  * [getIntervals()](#getintervals)
  * [getStudiesList()](#getstudieslist)
  * [undoRedoState()](#undoredostate)
* [Customization](#customization)
  * [changeTheme(themeName)](#changethemethemename)
  * [addCustomCSSFile(url)](#addcustomcssfileurl)
  * [applyOverrides(overrides)](#applyoverridesoverrides)
  * [applyStudiesOverrides(overrides)](#applystudiesoverridesoverrides)
* :chart: [Trading Terminal only](#chart-trading-terminal-only)
  * [watchList()](#chart-watchlist)
* :chart: [Multiple Charts Layout](#chart-multiple-charts-layout)
  * [chart(index)](#chart-chartindex)
  * [activeChart()](#chart-activechart)
  * [chartsCount()](#chart-chartscount)
  * [layout()](#chart-layout)
  * [setLayout(layout)](#chart-setlayoutlayout)
  * [layoutName()](#chart-layoutName)

## Subscribing To Chart Events

### onChartReady(callback)

1. `callback`: function()

The Charting Library will call the callback function 1 time when chart is initialized.
You can safely call all other methods starting from this moment.

### headerReady()

Returns a `Promise` object that should be used to handle an event when the Charting Library header widget API is ready (e.g. [createButton](#createbuttonoptions)).

### onGrayedObjectClicked(callback)

1. `callback`: function(subject)
    1. `subject`: object `{type, name}`
        * `type`: `drawing` | `study`
        * `name`: string, name of a clicked subject

The Library will call the `callback` function every time a user clicks on a grayed out object.

Example:

```javascript
new TradingView.widget({
    drawings_access: {
        type: "black",
        tools: [
            { name: "Trend Line" },
            { name: "Trend Angle", grayed: true },
        ]
    },
    studies_access: {
        type: "black",
        tools: [
            { name: "Aroon" },
            { name: "Balance of Power", grayed: true },
        ]
    },
    <...> // other widget settings
});

widget.onChartReady(function() {
    widget.onGrayedObjectClicked(function(data) {
        // this function will be called when a user tries to
        // create the Balance Of Power study or the Trend Angle shape

        alert(data.name + " is grayed out!");
    })
});
```

### onShortcut(shortcut, callback)

1. `shortcut`
1. `callback`: function(data)

The Library will call the `callback` function every time the shortcut key is pressed.

Example:

```javascript
widget.onShortcut("alt+s", function() {
  widget.executeActionById("symbolSearch");
});
```

### subscribe(event, callback)

1. `event`: can be

| Event name | Library Version | Description |
|------------|-----------------|-------------|
| `toggle_sidebar` | | Drawing toolbar is shown/hidden |
| `indicators_dialog` | | Indicators dialog is shown |
| `toggle_header` | | Chart header is shown/hidden |
| `edit_object_dialog` | | Chart/Study Properties dialog is shown |
| `chart_load_requested` | | New chart is about to be loaded |
| `chart_loaded` | | |
| `mouse_down` | | |
| `mouse_up` | | |
| `drawing` | 1.7 | A drawing is added to a chart. The arguments contain an object with the `value` field that corresponds with the name of the drawing. |
| `study` | 1.7 | An indicator is added to a chart.The arguments contain an object with the `value` field that corresponds with the name of the indicator. |
| `undo` | 1.7 | |
| `redo` | 1.7 | |
| `undo_redo_state_changed` | 1.14 | The Undo/Redo state has been changed. The arguments contain an object with the state of the Undo/Redo stack. This object has the same structure as the result of [UndoRedoState](Widget-Methods#undoredostate) method |
| `reset_scales` | 1.7 | Reset scales button is clicked |
| `compare_add` | 1.7 | A compare dialog is shown |
| `add_compare` | 1.7 | A compare instrument is added |
| `load_study` template | 1.7 | A study template is loaded |
| `onTick` | | Last bar is updated |
| `onAutoSaveNeeded` | | User changed the chart. `Chart change` means any user action that can be undone. The callback function will not be called more than once every 5 seconds. See also [auto_save_delay](Widget-Constructor#auto_save_delay) |
| `onScreenshotReady` | | A screenshot URL is returned by the server |
| `onMarkClick` | | User clicked a [mark on a bar](Marks-On-Bars). Mark ID will be passed as an argument |
| `onTimescaleMarkClick` | | User clicked a timescale mark. Mark ID will be passed as an argument |
| `onSelectedLineToolChanged` | | Selected line tool is changed |
| `study_properties_changed` | 1.14 | Study properties are changed. Entity ID will be passed as an argument |
| :chart: `layout_about_to_be_changed` | | Amount or placement of the charts is about to be changed |
| :chart: `layout_changed` | | Amount or placement of the charts is changed |
| :chart: `activeChartChanged` | | Active chart is changed |

1. `callback`: function(arguments)

The library will call the `callback` function when a GUI `event` has happened.
Every event can have a different set of arguments.

### unsubscribe(event, callback)

Unsubscribes a previously subscribed `callback` function from a given `event` (that is one of the events in the table above).

## Chart Actions

### chart()

Returns a chart object that you can use to call [Chart-Methods](Chart-Methods)

### setLanguage(locale)

1. `locale`: [language code](Localization)

Sets the language of the widget. For now, this call reloads the chart. **Please avoid using it**.

### setSymbol(symbol, interval, callback)

1. `symbol`: string
1. `interval`: string
1. `callback`: function()

Changes the symbol and resolution of the chart. The `callback` function is called only when new symbol's data has been received.

### remove()

Removes the chart widget from the web page.

### closePopupsAndDialogs()

Calling this method closes all context menus, pop-ups or dialogs.

### selectLineTool(drawingId)

1. `drawingId`: may be one of the [identifiers](Shapes-and-Overrides) or
    1. `cursor`
    1. `dot`
    1. `arrow_cursor`
    1. `eraser`
    1. `measure`
    1. `zoom`
    1. `brush`

Selects a drawing or a cursor. It's the same as a single click on a drawing button.

### selectedLineTool()

Returns an [identifier](Shapes-and-Overrides) of the selected drawing or cursor (see above).

### takeScreenshot()

This method creates a snapshot of the chart and uploads it to the server.
When it is done the [onScreenshotReady](#subscribeevent-callback) callback function is called.
The URL of the snapshot will be passed as an argument to the callback function.

### lockAllDrawingTools()

This method returns a [WatchedValue](WatchedValue) object that can be used to read/set/watch the state of Lock All Drawing Tools button.

### hideAllDrawingTools()

This method returns a [WatchedValue](WatchedValue) object that can be used to read/set/watch the state of Hide All Drawing Tools button.

## Saving/Loading Charts

### save(callback)

1. `callback`: function(object)

Saves the chart state to JS object. Charting Library will call your callback function and pass the state object as an argument.

This call is part of the low-level [save/load API](Saving-and-Loading-Charts).

### load(state)

1. `state`: object

Loads the chart from the `state` object. This call is part of the low-level [save/load API](Saving-and-Loading-Charts).

### getSavedCharts(callback)

1. `callback`: function(objects)

`objects` is an array of:

* `id`
* `name`
* `image_url`
* `modified_iso`
* `short_symbol`
* `interval`

Returns a list of chart descriptions saved to the server for the current user.

### loadChartFromServer(chartRecord)

1. `chartRecord` is an object that you get using [getSavedCharts(callback)](#getsavedchartscallback)

Loads and displays a chart from the server.

### saveChartToServer(onCompleteCallback, onFailCallback, saveAsSnapshot, options)

1. `onCompleteCallback`: function()
1. `onFailCallback`: function()
1. `saveAsSnapshot`: should be always `false`
1. `options`: object `{ chartName }`
    * `chartName`: name of a chart. Should be specified for new charts and when renaming the chart.
    * `defaultChartName`: default name of a chart. It will be used if the current chart has no name.

Saves the current chart to the server.

### removeChartFromServer(chartId, onCompleteCallback)

1. `chartId`: the `id` should be received from the object that is returned by the [getSavedCharts(callback)](#getsavedchartscallback)
1. `onCompleteCallback`: function()

Removes the chart from the server.

## Custom UI Controls

### onContextMenu(callback)

1. `callback`: function(unixtime, price).
    This callback function is expected to return a value (see below).

The Charting Library will call the callback function every time  user opens a context menu on the chart.
The arguments that are passed to the callback function contain unix time and price of the clicked point on the chart.

You have to return an array of objects that have the following format to add or remove items from the context menu.

```javascript
{
    position: 'top' | 'bottom',
    text: 'Menu item text',
    click: <onItemClicked callback>
}
```

* `position`: position of the item in the context menu
* `text`: menu item text
* `click`: a callback function that will be called when a user selects your menu item

Use the minus sign to add a separator. Example: `{ text: "-", position: "top" }`.

Use the minus sign in front of the item text to remove an existing item from the menu.

Example:

```javascript
widget.onChartReady(function() {
    widget.onContextMenu(function(unixtime, price) {
        return [{
            position: "top",
            text: "First top menu item, time: " + unixtime + ", price: " + price,
            click: function() { alert("First clicked."); }
        },
        { text: "-", position: "top" },
        { text: "-Objects Tree..." },
        {
            position: "top",
            text: "Second top menu item 2",
            click: function() { alert("Second clicked."); }
        }, {
            position: "bottom",
            text: "Bottom menu item",
            click: function() { alert("Third clicked."); }
        }];
    });
```

### createButton(options)

1. `options`: object `{ align: "left" }`
    * `align`: `right` | `left`. default: `left`

Creates a new DOM element in the top toolbar of the chart and returns [HTMLElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement) for this button.
You can use it to add custom controls right on the chart.

**NOTE:** This method MUST be called after [headerReady](#headerready) promise is resolved.

Example:

```javascript
widget.headerReady().then(function() {
    var button = widget.createButton();
    button.setAttribute('title', 'My custom button tooltip');
    button.addEventListener('click', function() { alert("My custom button pressed!"); });
    button.textContent = 'My custom button caption';
});
```

## Dialogs

*Starting from version 1.6.*

### showNoticeDialog(params)

1. `params`: object:
    * `title`: text to be shown in the title
    * `body`: text to be shown in the body
    * `callback`: function to be called when ok button is pressed

This method shows a dialog with custom title and text along with the "OK" button.

### showConfirmDialog(params)

1. `params`: object:
    * `title`: text to be shown in the title
    * `body`: text to be shown in the body
    * `callback(result)`: function to be called when ok button is pressed.
        `result` is `true` if `OK` is pressed, otherwise it is `false`.

This method shows a dialog with the custom title and text along with the "OK" and "CANCEL" buttons.

### showLoadChartDialog()

Displays the "Load chart layout" dialog.

### showSaveAsChartDialog()

Displays the "Copy chart layout" dialog.

## Getters

### symbolInterval()

Charting Library returns an object that contains the symbol and interval of the chart.

### mainSeriesPriceFormatter()

Returns an object with the `format` method that you can use to format the prices. This was introduced in version 1.5.

### getIntervals()

Returns an array of supported resolutions. This was introduced in version 1.7.

### getStudiesList()

Returns an array of IDs of all studies. They can be used to create a study.

### undoRedoState()

Returns an object with the state of the Undo/Redo stack. The object has the following keys:

    * `enableUndo`: boolean flag that shows the undo action availability
    * `undoText`: name of the next undo operation. If the undo stack is empty then it is undefined.
    * `enableRedo`: boolean flag that shows the redo action availability
    * `redoText`: name of the next redo operation. If the redo stack is empty then it is undefined.

## Customization

### changeTheme(themeName)

*Starting from version 1.13.*

1. `themeName` should be `"Light"` | `"Dark"`

This method changes the chart theme without reloading the chart.

You can also use the [theme](Widget-Constructor#theme) in the Widget Constructor to create the chart with a custom theme.

### addCustomCSSFile(url)

1. `url` should be an absolute or relative path to the 'static` folder

This method was introduced in version `1.3`.

Starting from version `1.4` use [custom_css_url](Widget-Constructor#custom_css_url) instead.

### applyOverrides(overrides)

*Starting from version 1.5.*

1. `overrides` is an object.
    It is the same as [overrides](Widget-Constructor#overrides) in the Widget Constructor.

This method applies "overrides" to the properties without reloading the chart.

### applyStudiesOverrides(overrides)

*Starting from version 1.9.*

1. `overrides` is an object. It is the same as [studies_overrides](Widget-Constructor#studies_overrides) in the Widget Constructor.

This method applies "overrides" to the styles or inputs of the indicators without reloading the chart.

## :chart: Trading Terminal only

The following methods are available in [Trading Terminal](Trading-Terminal) only.

### :chart: watchList()

*Starting from version 1.9.*

Returns an object to manage the watchlist. The object has the following methods:

1. `defaultList()` - allows you to get a default list of symbols.

1. `getList(id?: string)` - allows you to get a list of symbols. If the `id` parameter is not provided then the current list will be returned. If there is no WatchList then `null` will be returned.

1. `getActiveListId()` - allows you to get the ID of the current list. If there is no WatchList then `null` will be returned.

1. `getAllLists()` - allows you to get all lists. If there is no WatchList then `null` will be returned.

1. `setList(symbols: string[])` - allows you to set a list of symbols into the watchlist. It will replace the entire list. **Obsolete. Will be removed in version `1.13`. Use `updateList` instead.**

1. `updateList(listId: string, symbols: string[])` - allows you to edit the list of symbols.

1. `renameList(listId: string, newName: string)` - allows you to rename the list to `newName`.

1. `createList(listName?: string, symbols?: string[])` - allows you to create a list of symbols with `listName` name. If the `listName` parameter is not provided or there is no WatchList then `null` will be returned;

1. `saveList(list: SymbolList)` - allows you to save a list of symbols where `list` is an object with the following keys:

    ```js
    id: string;
    title: string;
    symbols: string[];
    ```

    If there is no WatchList or an equivalent list already exists then `false` will be returned, otherwise `true` will returned.

1. `deleteList(listId: string)` - allows you to delete a list of symbols.

1. `onListChanged()` - you can use this method to be notified when the symbols of the active watchlist are changed. You can subscribe and unsubscribe using the [[Subscription]] object returned by this function.

1. `onActiveListChanged()` - you can use this method to be notified when a different list of the watchlist is selected. You can subscribe and unsubscribe using the [[Subscription]] object returned by this function.

1. `onListAdded()` - - you can use this method to be notified when the new list is added to the watchlist. You can subscribe and unsubscribe using the [[Subscription]] object returned by this function.

1. `onListRemoved()` - you can use this method to be notified when the list is removed from the watchlist. You can subscribe and unsubscribe using the [[Subscription]] object returned by this function.

1. `onListRenamed()` - - you can use this method to be notified when the list is renamed in the watchlist. You can subscribe and unsubscribe using the [[Subscription]] object returned by this function.

## :chart: Multiple Charts Layout

### :chart: chart(index)

1. `index`: index of a chart starting from `0`. `index` is `0` by default.

Returns a chart object that you can use to call [Chart-Methods](Chart-Methods)

### :chart: activeChart()

Returns a chart object of the active chart that you can use to call [Chart-Methods](Chart-Methods)

### :chart: chartsCount()

Returns the amount of charts in the current layout.

### :chart: layout()

Returns the current layout mode. Possible values are: `4`, `6`, `8`, `s`, `2h`, `2-1`, `2v`, `3h`, `3v`, `3s`.

### :chart: setLayout(layout)

1. `layout`: Possible values are: `4`, `6`, `8`, `s`, `2h`, `2-1`, `2v`, `3h`, `3v`, `3s`.

Changes the current chart layout.

### :chart: layoutName()

Returns the current layout name. If the current layout has not yet been saved then it returns undefined.

## See Also

* [Chart-Methods](Chart-Methods)
* [Customization Overview](Customization-Overview)
* [Widget Constructor](Widget-Constructor)
* [Saving and Loading Charts](Saving-and-Loading-Charts)
* [Overriding Default Properties of the Studies](Studies-Overrides)
* [Overriding Default Properties of the Chart](Overrides)


# UDF

**What is UDF?** It's an HTTP-based protocol that is designed to deliver data to the Charting Library in a simple and efficient way.

**How can I start using it?** You should create a tiny server-side HTTP service that will get the data from your storage and respond to Charting Library requests.

## Response-as-a-table concept

Think of data feed responses as tables. For example, a data feed response that includes a symbol list from the exchange may be treated as a table where each symbol represents a row, along with some columns (minimal_price_movement, description, has_intraday e.t.c.).
Each column may be an array (it will provide a separate value for each row in a table).
Note that there might be a situation when all rows have the same value in the same column.
In this case, the column value can be defined as a single value in JSON response.

Example:

Let's assume that we requested a symbol list from the New York Stock Exchange. The response (in pseudo-format) might look like

```javascript
{
   symbols: ["MSFT", "AAPL", "FB", "GOOG"],
   min_price_move: 0.1,
   description: ["Microsoft corp.", "Apple Inc", "Facebook", "Google"]
}
```

Here is how this response will look like in a table format.

Symbol|min_price_move|Description
---|---|---
MSFT|0.1|Microsoft corp.
AAPL|0.1|Apple Inc
FB|0.1|Facebook
GOOG|0.1|Google

## API Calls

### Data feed configuration data

Request: `GET /config`

Response: Library expects to receive a JSON response of the same structure as a result of JS API [setup() call](JS-Api#onreadycallback).

Also there should be 2 additional properties:

* `supports_search`: Set it to `true` if your data feed supports symbol search and individual symbol resolve logic.
* `supports_group_request`: Set it to `true`  if your data feed provides full information on symbol group only and is not able to perform symbol search or individual symbol resolve.

Either `supports_search` or `supports_group_request` should be set to `true`.

**Remark**: If your data feed doesn't implement this call (doesn't respond or sends 404 error) then the default configuration is being used. Here is the default configuration:

```javascript
{
    supported_resolutions: ['1', '5', '15', '30', '60', '1D', '1W', '1M'],
    supports_group_request: true,
    supports_marks: false,
    supports_search: false,
    supports_timescale_marks: false,
}
```

### Symbol group request

Request: `GET /symbol_info?group=<group_name>`

1. `group_name`: string

Example: `GET /symbol_info?group=NYSE`

Response: Response is expected to be an object with properties listed below.
Each property is treated as table column, as described above (see [response-as-a-table](UDF#response-as-a-table-concept)).
The response structure is similar (but **not equal**) to [SymbolInfo](Symbology#symbolinfo-structure) so please read the description to learn about the details of each field.

* `symbol`
* `description`
* `exchange-listed` / `exchange-traded`
* `minmovement` / `minmov` (NOTE: `minmov` is deprecated and will be removed in future releases)
* `minmovement2` / `minmov2` (NOTE: `minmov2` is deprecated and will be removed in future releases)
* `fractional`
* `pricescale`
* `has-intraday`
* `has-no-volume`
* `type`
* `ticker`
* `timezone`
* `session-regular` (mapped to `SymbolInfo.session`)
* `supported-resolutions`
* `force-session-rebuild`
* `has-daily`
* `intraday-multipliers`
* `volume_precision`
* `has-weekly-and-monthly`
* `has-empty-bars`

Here is an example of data feed response to `GET /symbol_info?group=NYSE` request:

```javascript
{
   symbol: ["AAPL", "MSFT", "SPX"],
   description: ["Apple Inc", "Microsoft corp", "S&P 500 index"],
   exchange-listed: "NYSE",
   exchange-traded: "NYSE",
   minmovement: 1,
   minmovement2: 0,
   pricescale: [1, 1, 100],
   has-dwm: true,
   has-intraday: true,
   has-no-volume: [false, false, true]
   type: ["stock", "stock", "index"],
   ticker: ["AAPL~0", "MSFT~0", "$SPX500"],
   timezone: “America/New_York”,
   session-regular: “0900-1600”,
}
```

**Remark 1**: This call will be used if your data feed sent `supports_group_request: true` in the configuration data or didn't respond to the configuration request at all.

**Remark 2**: In the event that your data feed does not support the requested symbol group (which should not happen if your response to request #1 (supported groups) is correct) you may expect a 404 error.

**Remark 3**: When using this mode of receiving data (getting large amount of symbol data) your browser will keep the data that wasn't even requested by the user.
If your symbol list has more than a few items, please consider supporting symbol search / individual symbol resolve instead.

### Symbol resolve

Request: `GET /symbols?symbol=<symbol>`

1. `symbol`: string. Symbol name or ticker.

Example: `GET /symbols?symbol=AAL`, `GET /symbols?symbol=NYSE:MSFT`

A JSON response of the same structure as [SymbolInfo](Symbology#symbolinfo-structure)

**Remark**: This call will be requested if your data feed sent `supports_group_request: false` and `supports_search: true` in the configuration data.

### Symbol search

Request: `GET /search?query=<query>&type=<type>&exchange=<exchange>&limit=<limit>`

* `query`: string. Text typed by the user in the Symbol Search edit box
* `type`: string. One of the symbol types [supported](JS-Api#symbols_types) by your back-end
* `exchange`: string. One of the exchanges [supported](JS-Api#exchanges) by your back-end
* `limit`: integer. The maximum number of symbols in a response

Example: `GET /search?query=AA&type=stock&exchange=NYSE&limit=15`

A response is expected to be an array of symbol objects as in [respective JS API call](JS-Api#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback)

**Remark**: This call will be requested if your data feed sent `supports_group_request: false` and `supports_search: true` in the configuration data.

### Bars

Request: `GET /history?symbol=<ticker_name>&from=<unix_timestamp>&to=<unix_timestamp>&resolution=<resolution>`

* `symbol`: symbol name or ticker.
* `from`: unix timestamp (UTC) of leftmost required bar
* `to`: unix timestamp (UTC) of rightmost required bar
* `resolution`: string

Example: `GET /history?symbol=BEAM~0&resolution=D&from=1386493512&to=1395133512`

A response is expected to be an object with some properties listed below. Each property is treated as a table column, as described above.

* `s`: status code. Expected values: `ok` | `error` | `no_data`
* `errmsg`: Error message. Should be present only when `s = 'error'`
* `t`: Bar time. Unix timestamp (UTC)
* `c`: Closing price
* `o`: Opening price (optional)
* `h`: High price (optional)
* `l`: Low price (optional)
* `v`: Volume (optional)
* `nextTime`: Time of the next bar if there is no data (status code is `no_data`) in the requested period (optional)

**Remark**: Bar time for daily bars should be 00:00 UTC and is expected to be a trading day (not a day when the session starts).
Charting Library aligns the time according to the [Session](Symbology#session) from SymbolInfo.

**Remark**: Bar time for monthly bars should be 00:00 UTC and is the first trading day of the month.

**Remark**: Prices should be passed as numbers and not as strings in quotation marks.

Example:

```javascript
{
   s: "ok",
   t: [1386493512, 1386493572, 1386493632, 1386493692],
   c: [42.1, 43.4, 44.3, 42.8]
}
```

```javascript
{
   s: "no_data",
   nextTime: 1386493512
}
```

```javascript
{
   s: "ok",
   t: [1386493512, 1386493572, 1386493632, 1386493692],
   c: [42.1, 43.4, 44.3, 42.8],
   o: [41.0, 42.9, 43.7, 44.5],
   h: [43.0, 44.1, 44.8, 44.5],
   l: [40.4, 42.1, 42.8, 42.3],
   v: [12000, 18500, 24000, 45000]
}
```

#### How `nextTime` works

Let's assume that a user opened the chart where `resolution = 1` and the Library requests the following range of data from the data feed `[3 Apr 2015 16:00 UTC+0, 3 Apr 2015 19:00 UTC+0]` for a stock that is traded on the NYSE.
April 3rd was Good Friday which means that the markets were closed.
Library expects the following response from the data feed.

```javascript
{
  s: "no_data",
  nextTime: 1428001140000 // 2 Apr 2015 18:59:00 GMT+0
}
```

`nextTime` is the time of the closest available bar in the past.

### Marks

Request: `GET /marks?symbol=<ticker_name>&from=<unix_timestamp>&to=<unix_timestamp>&resolution=<resolution>`

* `symbol`: symbol name or ticker.
* `from`: unix timestamp (UTC) of leftmost visible bar
* `to`: unix timestamp (UTC) of rightmost visible bar
* `resolution`: string

A response is expected to be an object with some properties listed below.
This object is similar to [respective response](JS-Api#getmarkssymbolinfo-from-to-ondatacallback-resolution) in JS API, but each property is treated as a table column, as described above.

```javascript
{
    id: [array of ids],
    time: [array of times],
    color: [array of colors],
    text: [array of texts],
    label: [array of labels],
    labelFontColor: [array of label font colors],
    minSize: [array of minSizes],
}
```

**Remark**: This call will be requested if your data feed sent `supports_marks: true` in the configuration data.

### Timescale marks

Request: `GET /timescale_marks?symbol=<ticker_name>&from=<unix_timestamp>&to=<unix_timestamp>&resolution=<resolution>`

* `symbol`: symbol name or ticker.
* `from`: unix timestamp (UTC) or leftmost visible bar
* `to`: unix timestamp (UTC) or rightmost visible bar
* `resolution`: string

A response is expected to be an array of objects with properties listed below.

* `id`: unique identifier of a mark
* `color`: rgba color
* `label`: a letter to be displayed in a circle
* `time`: unix time
* `tooltip`: tooltip text

**Remark**: This call will be requested if your data feed sent `supports_timescale_marks: true` in the configuration data.

### Server time

Request: `GET /time`

Response: Numeric unix time without milliseconds.

Example: `1445324591`

### Quotes

Request: `GET /quotes?symbols=<ticker_name_1>,<ticker_name_2>,...,<ticker_name_n>`

Example: `GET /quotes?symbols=NYSE%3AAA%2CNYSE%3AF%2CNasdaqNM%3AAAPL`

A response is an object with the following keys.

* `s`: Status code for the request. Expected values are: `ok` or `error`
* `errmsg`: Error message. Should be present only when `s = 'error'`
* `d`: [symbols data](Quotes) Array

Example:

```json
{
    "s": "ok",
    "d": [
        {
            "s": "ok",
            "n": "NYSE:AA",
            "v": {
                "ch": "+0.16",
                "chp": "0.98",
                "short_name": "AA",
                "exchange": "NYSE",
                "description": "Alcoa Inc. Common",
                "lp": "16.57",
                "ask": "16.58",
                "bid": "16.57",
                "open_price": "16.25",
                "high_price": "16.60",
                "low_price": "16.25",
                "prev_close_price": "16.41",
                "volume": "4029041"
            }
        },
        {
            "s": "ok",
            "n": "NYSE:F",
            "v": {
                "ch": "+0.15",
                "chp": "0.89",
                "short_name": "F",
                "exchange": "NYSE",
                "description": "Ford Motor Company",
                "lp": "17.02",
                "ask": "17.03",
                "bid": "17.02",
                "open_price": "16.74",
                "high_price": "17.08",
                "low_price": "16.74",
                "prev_close_price": "16.87",
                "volume": "7713782"
            }
        }
    ]
}
```

## Constructor

`Datafeeds.UDFCompatibleDatafeed = function(datafeedURL, updateFrequency)`

### datafeedURL

This is a URL of a data server that will receive requests and return data.

### updateFrequency

This is a frequency of requests that the data feed will send to the server in milliseconds. Default is 10000 (10 sec).

#JS-Api

**What is JS API?**: A set of JS methods (specific public interface).

**Which steps should I follow to start using it?**: You should create a JS object that will receive data by some way and respond to Charting Library requests.

Data caching (history & symbol info) is implemented in the Charting Library.

When you create an object that implements the described interface simply pass it to Library widget constructor through [`datafeed` argument](Widget-Constructor#datafeed).

## Methods

1. [onReady](#onreadycallback)
1. [searchSymbols](#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback)
1. [resolveSymbol](#resolvesymbolsymbolname-onsymbolresolvedcallback-onresolveerrorcallback)
1. [getBars](#getbarssymbolinfo-resolution-from-to-onhistorycallback-onerrorcallback-firstdatarequest)
1. [subscribeBars](#subscribebarssymbolinfo-resolution-onrealtimecallback-subscriberuid-onresetcacheneededcallback)
1. [unsubscribeBars](#unsubscribebarssubscriberuid)
1. [calculateHistoryDepth](#calculatehistorydepthresolution-resolutionback-intervalback)
1. [getMarks](#getmarkssymbolinfo-from-to-ondatacallback-resolution)
1. [getTimescaleMarks](#gettimescalemarkssymbolinfo-from-to-ondatacallback-resolution)
1. [getServerTime](#getservertimecallback)

:chart: [Trading Terminal](Trading-Terminal) specific:

1. [getQuotes](#getquotessymbols-ondatacallback-onerrorcallback)
1. [subscribeQuotes](#subscribequotessymbols-fastsymbols-onrealtimecallback-listenerguid)
1. [unsubscribeQuotes](#unsubscribequoteslistenerguid)
1. [subscribeDepth](#subscribedepthsymbolinfo-callback-string)
1. [unsubscribeDepth](#unsubscribedepthsubscriberuid)

### onReady(callback)

1. `callback`: function(configurationData)
    1. `configurationData`: object (see below)

This call is intended to provide the object filled with the configuration data.
This data partially affects the chart behavior and is called [server-side customization](Customization-Overview#customization-done-through-data-stream).
Charting Library assumes that you will call the callback function and pass your datafeed `configurationData` as an argument.
Configuration data is an object; for now, the following properties are supported:

#### exchanges

An array of exchange descriptors. Exchange descriptor is an object `{value, name, desc}`. `value` will be passed as `exchange` argument to [searchSymbols](#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback).

`exchanges = []`  leads to the absence of the exchanges filter in Symbol Search list. Use `value = ""` if you wish to include all exchanges.

#### symbols_types

An array of filter descriptors. Filter descriptor is an object `{name, value}`. `value` will be passed as `symbolType` argument to [searchSymbols](#searchsymbolsuserinput-exchange-symboltype-onresultreadycallback).

`symbolsTypes = []`  leads to the absence of filter types in Symbol Search list. Use `value = ""` if you wish to include all filter types.

#### supported_resolutions

An array of supported resolutions. Resolution must be a string. Format is described in another [article](Resolution).

`supported_resolutions = undefined` or `supported_resolutions = []` leads to resolution widget including the default content.

Example: `["1", "15", "240", "D", "6M"]` will give you "1 minute, 15 minutes, 4 hours, 1 day, 6 months" in resolution widget.

#### supports_marks

Boolean showing whether your datafeed supports marks on bars or not.

#### supports_timescale_marks

Boolean showing whether your datafeed supports timescale marks or not.

#### supports_time

Set this one to `true` if your datafeed provides server time (unix time). It is used to adjust Countdown on the Price scale.

#### futures_regex

Set it if you want to group futures in the symbol search. This REGEX should divide an instrument name into 2 parts: a root and an expiration.

Sample regex: : `/^(.+)([12]!|[FGHJKMNQUVXZ]\d{1,2})$/`. It will be applied to the instruments with `futures` as a `type`.

### searchSymbols(userInput, exchange, symbolType, onResultReadyCallback)

1. `userInput`: string. It is text entered by user in the symbol search field.
1. `exchange`: string. The requested exchange (chosen by user). Empty value means no filter was specified.
1. `symbolType`: string. The requested symbol type: `index`, `stock`, `forex`, etc (chosen by user).
    Empty value means no filter was specified.
1. `onResultReadyCallback`: function(result)
    1. `result`: array (see below)

This call is intended to provide the list of symbols that match the user's search query. `result` is expected to look like the following:

```javascript
[
    {
        "symbol": "<short symbol name>",
        "full_name": "<full symbol name>", // e.g. BTCE:BTCUSD
        "description": "<symbol description>",
        "exchange": "<symbol exchange name>",
        "ticker": "<symbol ticker name, optional>",
        "type": "stock" // or "futures" or "bitcoin" or "forex" or "index"
    },
    {
        //    .....
    }
]
```

If no symbols are found, then callback should be called with an empty array. See more details about `ticker` value [here](Symbology#ticker)

### resolveSymbol(symbolName, onSymbolResolvedCallback, onResolveErrorCallback)

1. `symbolName`: string. Symbol name or `ticker` if provided.
1. `onSymbolResolvedCallback`: function([SymbolInfo](Symbology#symbolinfo-structure))
1. `onResolveErrorCallback`: function(reason)

Charting Library will call this function when it needs to get [SymbolInfo](Symbology#symbolinfo-structure) by symbol name.

### getBars(symbolInfo, resolution, from, to, onHistoryCallback, onErrorCallback, firstDataRequest)

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `resolution`: string
1. `from`: unix timestamp, leftmost required bar time
1. `to`: unix timestamp, rightmost required bar time
1. `onHistoryCallback`: function(array of `bar`s, `meta` = `{ noData = false }`)
    1. `bar`: object `{time, close, open, high, low, volume}`
    1. `meta`: object `{noData = true | false, nextTime - unix time}`
1. `onErrorCallback`: function(reason)
1. `firstDataRequest`: boolean to identify the first call of this method for the particular symbol resolution.
    When it is set to `true` you can ignore `to` (which depends on browser's `Date.now()`) and return bars up to the latest bar.

This function is called when the chart needs a history fragment defined by dates range.

The charting library assumes `onHistoryCallback` to be called **just once**.

**Important**: `nextTime` is a time of the next bar in the history. It should be set if the requested period represents a gap in the data. Hence there is available data prior to the requested period.

**Important**: `noData` should be set if there is no data in the requested period.

**Remark**: `bar.time` is expected to be the amount of milliseconds since Unix epoch start in **UTC** timezone.

**Remark**: `bar.time` for daily bars is expected to be a trading day (not session start day) at 00:00 UTC.
Charting Library adjusts time according to [Session](Symbology#session) from SymbolInfo

**Remark**: `bar.time` for monthly bars is the first trading day of the month without the time part

### subscribeBars(symbolInfo, resolution, onRealtimeCallback, subscriberUID, onResetCacheNeededCallback)

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `resolution`: string
1. `onRealtimeCallback`: function(bar)
    1. `bar`: object `{time, close, open, high, low, volume}`
1. `subscriberUID`: object
1. `onResetCacheNeededCallback` *(since version 1.7)*: function() to be executed when bar data has changed

Charting Library calls this function when it wants to receive real-time updates for a symbol. The Library assumes that you will call `onRealtimeCallback` every time you want to update the most recent bar or to add a new one.

**Remark**: When you call `onRealtimeCallback` with bar having time equal to the most recent bar's time then the entire last bar is replaced with the `bar` object you've passed into the call.

Example:

1. The most recent bar is `{1419411578413, 10, 12, 9, 11}`
1. You call `onRealtimeCallback({1419411578413, 10, 14, 9, 14})`
1. Library finds out that the bar with the time `1419411578413` already exists and is the most recent one
1. Library replaces the entire bar making the most recent bar `{1419411578413, 10, 14, 9, 14}`

**Remark 2**: Is it possible either to update the most recent bar or to add a new one with `onRealtimeCallback`.
You'll get an error if you call this function when trying to update a historical bar.

**Remark 3**: There is no way to change historical bars once they've been received by the chart currently.

### unsubscribeBars(subscriberUID)

1. `subscriberUID`: object

Charting Library calls this function when it doesn't want to receive updates for this subscriber any more. `subscriberUID` will be the same object that Library passed to `subscribeBars` before.

### calculateHistoryDepth(resolution, resolutionBack, intervalBack)

*Optional.*

1. `resolution`: requested symbol resolution
1. `resolutionBack`: time period types. Supported values are: `D` | `M`
1. `intervalBack`: amount of `resolutionBack` periods that the Charting Library is going to request

Charting Library calls this function when it is going to request some historical data to give you an ability to override the amount of bars requested.

It passes some arguments so that you are aware of the amount of bars it's going to get. Here are some examples:

* `calculateHistoryDepth("D", "M", 12)` called: the Library is going to request 12 months of daily bars
* `calculateHistoryDepth("60", "D", 15)` called: the Library is going to request 15 days of hourly bars

This function should return `undefined` if you do not wish to override anything.
If you do, it should return an object `{resolutionBack, intervalBack}`.

Example:

Let's assume that the implementation is as follows

```javascript
Datafeed.prototype.calculateHistoryDepth = function(resolution, resolutionBack, intervalBack) {
    if (resolution === "1D") {
        return {
            resolutionBack: 'M',
            intervalBack: 6
        };
    }
}
```

When the Charting Library requests the data for `1D` resolution, the history will be 6 months deep.
In all other cases the history depth will have the default value.

### getMarks(symbolInfo, from, to, onDataCallback, resolution)

*Optional.*

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `from`: unix timestamp (UTC). Leftmost visible bar's time.
1. `to`: unix timestamp (UTC). Rightmost visible bar's time.
1. `onDataCallback`: function(array of `mark`s)
1. `resolution`: string

The Library calls this function to get [marks](Marks-On-Bars) for visible bars range.

The Library assumes that you will call `onDataCallback` only once per `getMarks` call.

`mark` is an object that has the following properties:

* `id`: unique mark ID. It will be passed to a [respective callback](Widget-Methods#subscribeevent-callback) when user clicks on a mark
* `time`: unix time, UTC
* `color`: `red` | `green` | `blue` | `yellow` | `{ border: '#ff0000', background: '#00ff00' }`
* `text`: mark popup text. HTML supported
* `label`: a letter to be printed on a mark. Single character
* `labelFontColor`: color of a letter on a mark
* `minSize`: minimum mark size (diameter, pixels) (default value is `5`)

A few marks per bar are allowed (for now, the maximum is `10`). Marks outside of the bars are not allowed.

**Remark**: This function will be called only if you confirmed that your back-end is [supporting marks](#supports_marks).

### getTimescaleMarks(symbolInfo, from, to, onDataCallback, resolution)

*Optional.*

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `from`: unix timestamp (UTC). Leftmost visible bar's time.
1. `to`: unix timestamp (UTC). Rightmost visible bar's time.
1. `onDataCallback`: function(array of `mark`s)
1. `resolution`: string

The Library calls this function to get timescale marks for visible bars range.

The Library assumes that you will call `onDataCallback` only once per `getTimescaleMarks` call.

`mark` is an object that has the following properties:

* `id`: unique mark ID. Will be passed to a [respective callback](Widget-Methods#subscribeevent-callback) when user clicks on a mark
* `time`: unix time, UTC
* `color`: `red` | `green` | `blue` | `yellow` | ... | `#000000`
* `label`: a letter to be printed on a mark. Single character
* `tooltip`: array of text strings. Each element of the array is a new text line of a tooltip.

Only one mark per bar is allowed. Marks outside of the bars are not allowed.

**Remark**: This function will be called only if you confirmed that your back-end is [supporting marks](#supports_timescale_marks).

### getServerTime(callback)

1. `callback`: function(unixTime)

This function is called if the configuration flag `supports_time` is set to `true` when the Charting Library needs to know the server time.

The Charting Library assumes that the callback is called once.

The time is provided without milliseconds.

It is used to display Countdown on the price scale.

Example: `1445324591`.

## [Trading Terminal](Trading-Terminal) specific

### getQuotes(symbols, onDataCallback, onErrorCallback)

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `symbols`: array of symbols names
1. `onDataCallback`: function(array of `data`)
    1. `data`: [symbol quote data](Quotes#symbol-quote-data)
1. `onErrorCallback`: function(reason)

This function is called when the Charting Library needs quote data. The charting library assumes that `onDataCallback` is called once when all the requested data is received.

### subscribeQuotes(symbols, fastSymbols, onRealtimeCallback, listenerGUID)

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `symbols`: array of symbols that should be updated rarely (once per minute). These symbols are included in the watchlist but they are not visible at the moment.
1. `fastSymbols`: array of symbols that should be updated frequently (once every 10 seconds or more often)
1. `onRealtimeCallback`: function(array of `data`)
    1. `data`: [symbol quote data](Quotes#symbol-quote-data)
1. `listenerGUID`: unique identifier of the listener

Trading Terminal calls this function when it wants to receive real-time quotes for a symbol.

The Charting Library assumes that you will call `onRealtimeCallback` every time you want to update the quotes.

### unsubscribeQuotes(listenerGUID)

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `listenerGUID`: unique identifier of the listener

Trading Terminal calls this function when it doesn't want to receive updates for this listener anymore.

`listenerGUID` will be the same object that the Library passed to `subscribeQuotes` before.

### subscribeDepth(symbolInfo, callback): string

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `symbolInfo`: [SymbolInfo](Symbology#symbolinfo-structure) object
1. `callback`: function(depth)
    1. `depth`: object `{snapshot, asks, bids}`
        * `snapshot`: Boolean - if `true` `asks` and `bids` have full set of depth, otherwise they contain only updated levels.
        * `asks`: Array of `{price, volume}` (must be sorted by `price` in asc order)
        * `bids`: Array of `{price, volume}` (must be sorted by `price` in asc order)

Trading Terminal calls this function when it wants to receive real-time level 2 (DOM) for a symbol. The Charting Library assumes that you will call the `callback` every time you want to update DOM data.

This method should return a unique identifier (`subscriberUID`) that will be used to unsubscribe from the data.

### unsubscribeDepth(subscriberUID)

:chart: *[Trading Terminal](Trading-Terminal) specific.*

1. `subscriberUID`: String

Trading Terminal calls this function when it doesn't want to receive updates for this listener anymore.

`subscriberUID` will be the same object that was returned from `subscribeDepth`.



# Resolution

Resolution or time interval is a time period of one bar. The Charting Library supports intraday resolutions (seconds, minutes, hours) and DWM resolutions (daily, weekly, monthly).
Charting Library API has lots of methods that accept and return resolutions.

## Intraday

### Seconds

Format: `xS`, where `x` is a number of seconds.

Example: `1S` - one second, `2S` - two seconds, `100S` - one hundred seconds.

### Minutes

Format: `x`, where `x` is a number of minutes.

Example: `1` - one minute, `2` - two minutes, `100` - one hundred minutes.

### Hours

**Important:** while user interface allows a user to enter a number of hours as `xh` or `xH`, it is never passed to the API. Hours are always set using minutes in the Charting Library API.

Example: `60` - one hour, `120` - two hours, `240` - four hours.

## DWM

### Days

Format: `xD`, where `x` is a number of days.

Example: `1D` - one day, `2D` - two days, `100D` - one hundred days.

### Weeks

Format: `xW`, where `x` is a number of weeks.

Example: `1W` - one week, `2W` - two weeks, `100W` - one hundred weeks.

### Months

Format: `xM`, where `x` is a number of months.

Example: `1M` - one month, `2M` - two months, `100M` - one hundred months.

### Years

Years are set using months.

Example: `12M` - one year, `24M` - two year, `48M` - four years.

## See also

* [How to set a list of available resolutions on a chart](JS-Api#supported_resolutions)
* [How to set a list of resolutions supported by the financial instrument](Symbology#supported_resolutions)
* [Set initial resolution on a chart](Widget-Constructor#symbol-interval)
* [Get current chart resolution](Chart-Methods#resolution)
* [Change resolution on a chart](Chart-Methods#setresolutionresolution-callback)


# Time-Frames

You can see the toolbar at the bottom of the chart. Each of those buttons on the left side switches the chart time frame. Switching time frame means:

1. Switch the chart resolution
1. Force the bars to scale horizontally in order to cover the entire requested date/time range.

I.e., clicking `1Y` will make the chart switch the resolution to `1D` and scale it accordingly to display daily bars for the entire year. Each time frame has its own chart resolution. Here it the list of default time frames:

Time Frame|Chart Resolution
---|---
5Y|W
1Y|D
6M|120
3M|60
1M|30
5D|5
1D|1

You can customize default time frames using the [time_frames](Widget-Constructor#time_frames) argument of the widget constructor.

**Remark**: Time frames that require resolutions which are not available for a particular symbol will be hidden.

# Drawings-Overrides

The complete list of drawings overrides with default values is presented here. You can change the default values using the `overrides` argument of the [widget constructor](Widget-Constructor#overrides). At the bottom of this list you will find a list of constants and abbreviations used in the values.

```javascript
linetoolicon: {
    singleChartOnly: true,
    color: 'rgba( 61, 133, 198, 1)',
    snapTo45Degrees:true,
    size: 40,
    icon: 0x263A,
    angle: Math.PI * 0.5,
    scale: 1.0
},
linetoolbezierquadro: {
    linecolor: 'rgba( 21, 153, 128, 1)',
    linewidth: 1.0,
    fillBackground: false,
    backgroundColor: 'rgba( 21, 56, 153, 0.5)',
    transparency: 50,
    linestyle: LINESTYLE_SOLID,
    extendLeft: false,
    extendRight: false,
    leftEnd: LINEEND_NORMAL,
    rightEnd: LINEEND_NORMAL
},
linetoolbeziercubic: {
    linecolor: 'rgba( 21, 153, 128, 1)',
    linewidth: 1.0,
    fillBackground: false,
    backgroundColor: 'rgba( 21, 56, 153, 0.5)',
    transparency: 50,
    linestyle: LINESTYLE_SOLID,
    extendLeft: false,
    extendRight: false,
    leftEnd: LINEEND_NORMAL,
    rightEnd: LINEEND_NORMAL
},
linetooltrendline: {
    linecolor: 'rgba( 21, 153, 128, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    extendLeft: false,
    extendRight: false,
    leftEnd: LINEEND_NORMAL,
    rightEnd: LINEEND_NORMAL,
    font: 'Verdana',
    textcolor: 'rgba( 21, 119, 96, 1)',
    fontsize: 12,
    bold:false,
    italic:false,
    snapTo45Degrees:true,
    alwaysShowStats: false,
    showPriceRange: false,
    showBarsRange: false,
    showDateTimeRange: false,
    showDistance:false,
    showAngle: false
},
linetoolinfoline: {
    clonable: true,
    linecolor: 'rgba( 21, 153, 128, 1)',
    linewidth: 1.0,
    linestyle: CanvasEx.LINESTYLE_SOLID,
    extendLeft: false,
    extendRight: false,
    leftEnd: LineEnd.Normal,
    rightEnd: LineEnd.Normal,
    font: 'Verdana',
    textcolor: 'rgba( 21, 119, 96, 1)',
    fontsize: 12,
    bold: false,
    italic: false,
    snapTo45Degrees: true,
    alwaysShowStats: true,
    showMiddlePoint: false,
    showPriceRange: true,
    showBarsRange: true,
    showDateTimeRange: true,
    showDistance: true,
    showAngle: true,
    statsPosition: 1,
},
linetooltimecycles: {
    linecolor: 'rgba(21, 153, 128, 1)',
    linewidth: 1.0,
    fillBackground: true,
    backgroundColor: 'rgba(106, 168, 79, 0.5)',
    transparency: 50,
    linestyle: LINESTYLE_SOLID
},
linetoolsineline: {
    linecolor: 'rgba( 21, 153, 128, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID
},
linetooltrendangle: {
    singleChartOnly: true,
    linecolor: 'rgba( 21, 153, 128, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    snapTo45Degrees:true,
    font: 'Verdana',
    textcolor: 'rgba( 21, 119, 96, 1)',
    fontsize: 12,
    bold:true,
    italic:false,
    alwaysShowStats: false,
    showPriceRange: false,
    showBarsRange: false,
    extendRight: false,
    extendLeft: false
},
linetooldisjointangle: {
    linecolor: 'rgba( 18, 159, 92, 1)',
    linewidth: 2.0,
    linestyle: LINESTYLE_SOLID,
    fillBackground: true,
    backgroundColor: 'rgba( 106, 168, 79, 0.5)',
    transparency: 50,
    extendLeft: false,
    extendRight: false,
    leftEnd: LINEEND_NORMAL,
    rightEnd: LINEEND_NORMAL,
    font: 'Verdana',
    textcolor: 'rgba( 18, 159, 92, 1)',
    fontsize: 12,
    bold:false,
    italic:false,
    showPrices: false,
    showPriceRange: false,
    showDateTimeRange: false,
    showBarsRange: false
},
linetoolflatbottom: {
    linecolor: 'rgba( 73, 133, 231, 1)',
    linewidth: 2.0,
    linestyle: LINESTYLE_SOLID,
    fillBackground: true,
    backgroundColor: 'rgba( 21, 56, 153, 0.5)',
    transparency: 50,
    extendLeft: false,
    extendRight: false,
    leftEnd: LINEEND_NORMAL,
    rightEnd: LINEEND_NORMAL,
    font: 'Verdana',
    textcolor: 'rgba( 73, 133, 231, 1)',
    fontsize: 12,
    bold:false,
    italic:false,
    showPrices: false,
    showPriceRange: false,
    showDateTimeRange: false,
    showBarsRange: false
},
linetoolfibspiral: {
    linecolor: 'rgba( 21, 153, 128, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID
},
linetooldaterange: {
    linecolor: 'rgba( 88, 88, 88, 1)',
    linewidth: 1.0,
    font: 'Verdana',
    textcolor: 'rgba( 255, 255, 255, 1)',
    fontsize: 12,
    fillLabelBackground: true,
    labelBackgroundColor: 'rgba( 91, 133, 191, 0.9)',
    labelBackgroundTransparency: 30,
    fillBackground: true,
    backgroundColor: 'rgba( 186, 218, 255, 0.4)',
    backgroundTransparency: 60,
    drawBorder: false,
    borderColor: 'rgba( 102, 123, 139, 1)'
},
linetoolpricerange: {
    linecolor: 'rgba( 88, 88, 88, 1)',
    linewidth: 1.0,
    font: 'Verdana',
    textcolor: 'rgba( 255, 255, 255, 1)',
    fontsize: 12,
    fillLabelBackground: true,
    labelBackgroundColor: 'rgba( 91, 133, 191, 0.9)',
    labelBackgroundTransparency: 30,
    fillBackground: true,
    backgroundColor: 'rgba( 186, 218, 255, 0.4)',
    backgroundTransparency: 60,
    drawBorder: false,
    borderColor: 'rgba( 102, 123, 139, 1)'
},
linetooldateandpricerange: {
    linecolor: 'rgba( 88, 88, 88, 1)',
    linewidth: 1.0,
    font: 'Verdana',
    textcolor: 'rgba( 255, 255, 255, 1)',
    fontsize: 12,
    fillLabelBackground: true,
    labelBackgroundColor: 'rgba( 91, 133, 191, 0.9)',
    labelBackgroundTransparency: 30,
    fillBackground: true,
    backgroundColor: 'rgba( 186, 218, 255, 0.4)',
    backgroundTransparency: 60,
    drawBorder: false,
    borderColor: 'rgba( 102, 123, 139, 1)'
},
linetoolriskrewardshort: {
    linecolor: 'rgba( 88, 88, 88, 1)',
    linewidth: 1.0,
    font: 'Verdana',
    textcolor: 'rgba(255, 255, 255, 1)',
    fontsize: 12,
    fillLabelBackground: true,
    labelBackgroundColor: 'rgba( 88, 88, 88, 1)',
    labelBackgroundTransparency: 0,
    fillBackground: true,
    stopBackground: 'rgba( 255, 0, 0, 0.2)',
    profitBackground: 'rgba( 0, 160, 0, 0.2)',
    stopBackgroundTransparency: 80,
    profitBackgroundTransparency: 80,
    drawBorder: false,
    borderColor: 'rgba( 102, 123, 139, 1)'
},
linetoolriskrewardlong: {
    linecolor: 'rgba( 88, 88, 88, 1)',
    linewidth: 1.0,
    font: 'Verdana',
    textcolor: 'rgba(255, 255, 255, 1)',
    fontsize: 12,
    fillLabelBackground: true,
    labelBackgroundColor: 'rgba( 88, 88, 88, 1)',
    labelBackgroundTransparency: 0,
    fillBackground: true,
    stopBackground: 'rgba( 255, 0, 0, 0.2)',
    profitBackground: 'rgba( 0, 160, 0, 0.2)',
    stopBackgroundTransparency: 80,
    profitBackgroundTransparency: 80,
    drawBorder: false,
    borderColor: 'rgba( 102, 123, 139, 1)'
},
linetoolarrow: {
    linecolor: 'rgba( 111, 136, 198, 1)',
    linewidth: 2.0,
    linestyle: LINESTYLE_SOLID,
    extendLeft: false,
    extendRight: false,
    leftEnd: LINEEND_NORMAL,
    rightEnd: LINEEND_ARROW,
    font: 'Verdana',
    textcolor: 'rgba( 21, 119, 96, 1)',
    fontsize: 12,
    bold:false,
    italic:false,
    alwaysShowStats: false,
    showPriceRange: false,
    showBarsRange: false,
    showDateTimeRange: false,
    showDistance:false,
    showAngle: false
},
linetoolray: {
    linecolor: 'rgba( 21, 153, 128, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    extendLeft: false,
    extendRight: true,
    leftEnd: LINEEND_NORMAL,
    rightEnd: LINEEND_NORMAL,
    font: 'Verdana',
    textcolor: 'rgba( 21, 119, 96, 1)',
    fontsize: 12,
    bold:false,
    italic:false,
    alwaysShowStats: false,
    showPriceRange: false,
    showBarsRange: false,
    showDateTimeRange: false,
    showDistance:false,
    showAngle: false
},
linetoolextended: {
    linecolor: 'rgba( 21, 153, 128, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    extendLeft: true,
    extendRight: true,
    leftEnd: LINEEND_NORMAL,
    rightEnd: LINEEND_NORMAL,
    font: 'Verdana',
    textcolor: 'rgba( 21, 119, 96, 1)',
    fontsize: 12,
    bold:false,
    italic:false,
    alwaysShowStats: false,
    showPriceRange: false,
    showBarsRange: false,
    showDateTimeRange: false,
    showDistance:false,
    showAngle: false
},
linetoolhorzline: {
    linecolor: 'rgba( 128, 204, 219, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    showPrice: true,
    showLabel: false,
    text: '',
    font: 'Verdana',
    textcolor: 'rgba( 21, 119, 96, 1)',
    fontsize: 12,
    bold:false,
    italic:false,
    horzLabelsAlign: 'center',
    vertLabelsAlign: 'top'
},
linetoolhorzray: {
    linecolor: 'rgba( 128, 204, 219, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    showPrice: true,
    showLabel: false,
    text: '',
    font: 'Verdana',
    textcolor: 'rgba( 21, 119, 96, 1)',
    fontsize: 12,
    bold:false,
    italic:false,
    horzLabelsAlign: 'center',
    vertLabelsAlign: 'top'
},
linetoolvertline: {
    linecolor: 'rgba( 128, 204, 219, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    showTime: true
},
linetoolcrossline: {
    clonable: true,
    linecolor: 'rgba(6, 160, 227, 1)',
    linewidth: 1.0,
    linestyle: CanvasEx.LINESTYLE_SOLID,
    showPrice: true,
    showTime: true
},
linetoolcirclelines: {
    trendline: {
        visible: true,
        color: 'rgba( 128, 128, 128, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_DASHED
    },
    linecolor: 'rgba( 128, 204, 219, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID
},
linetoolfibtimezone: {
    horzLabelsAlign: 'right',
    vertLabelsAlign: 'bottom',
    baselinecolor: 'rgba( 128, 128, 128, 1)',
    linecolor: 'rgba( 0, 85, 219, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    showLabels: true,
    font: 'Verdana',
    fillBackground:false,
    transparency:80,
    trendline: {
        visible: true,
        color: 'rgba( 128, 128, 128, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_DASHED
    },
    level1-11: LEVELS_TYPE_C
},
linetooltext: {
    color: 'rgba( 102, 123, 139, 1)',
    text: $.t('Text'),
    font: 'Verdana',
    fontsize: 20,
    fillBackground: false,
    backgroundColor: 'rgba( 91, 133, 191, 0.9)',
    backgroundTransparency: 70,
    drawBorder: false,
    borderColor: 'rgba( 102, 123, 139, 1)',
    bold:false,
    italic:false,
    locked: false,
    fixedSize: true,
    wordWrap: false,
    wordWrapWidth: 400
},
linetooltextabsolute: {
    singleChartOnly: true,
    color: 'rgba( 102, 123, 139, 1)',
    text: $.t('Text'),
    font: 'Verdana',
    fontsize: 20,
    fillBackground: false,
    backgroundColor: 'rgba( 155, 190, 213, 0.3)',
    backgroundTransparency: 70,
    drawBorder: false,
    borderColor: 'rgba( 102, 123, 139, 1)',
    bold: false,
    italic: false,
    locked: true,
    wordWrap: false,
    wordWrapWidth: 400
},
linetoolballoon: {
    color: 'rgba( 102, 123, 139, 1)',
    backgroundColor: 'rgba( 255, 254, 206, 0.7)',
    borderColor: 'rgba( 140, 140, 140, 1)',
    fontWeight: 'bold',
    fontsize: 12,
    font: 'Arial',
    transparency: 30,
    text: $.t('Comment')
},
linetoolbrush: {
    linecolor: 'rgba( 53, 53, 53, 1)',
    linewidth: 2.0,
    smooth:5,
    fillBackground: false,
    backgroundColor: 'rgba( 21, 56, 153, 0.5)',
    transparency: 50,
    leftEnd: LINEEND_NORMAL,
    rightEnd: LINEEND_NORMAL
},
linetoolpolyline: {
    linecolor: 'rgba( 53, 53, 53, 1)',
    linewidth: 2.0,
    linestyle: LINESTYLE_SOLID,
    fillBackground: true,
    backgroundColor: 'rgba( 21, 56, 153, 0.5)',
    transparency: 50,
    filled: false
},
linetoolarrowmark: {
    color: 'rgba( 120, 120, 120, 1)',
    text: '',
    fontsize: 20,
    font: 'Verdana'
},
linetoolarrowmarkleft: {
    color: 'rgba( 120, 120, 120, 1)',
    text: '',
    fontsize: 20,
    font: 'Verdana'
},
linetoolarrowmarkup: {
    color: 'rgba( 120, 120, 120, 1)',
    text: '',
    fontsize: 20,
    font: 'Verdana'
},
linetoolarrowmarkright: {
    color: 'rgba( 120, 120, 120, 1)',
    text: '',
    fontsize: 20,
    font: 'Verdana'
},
linetoolarrowmarkdown: {
    color: 'rgba( 120, 120, 120, 1)',
    text: '',
    fontsize: 20,
    font: 'Verdana'
},
linetoolflagmark: {
    color: 'rgba( 255, 0, 0, 1)'
},

linetoolnote: {
    markerColor: 'rgba( 46, 102, 255, 1)',
    textColor: 'rgba( 0, 0, 0, 1)',
    backgroundColor: 'rgba( 255, 255, 255, 1)',
    backgroundTransparency: 0,
    text: 'Text',
    font: 'Arial',
    fontSize: 12,
    bold: false,
    italic: false,
    locked: false,
    fixedSize: true
},

linetoolnoteabsolute: {
    singleChartOnly: true,
    markerColor: 'rgba( 46, 102, 255, 1)',
    textColor: 'rgba( 0, 0, 0, 1)',
    backgroundColor: 'rgba( 255, 255, 255, 1)',
    backgroundTransparency: 0,
    text: 'Text',
    font: 'Arial',
    fontSize: 12,
    bold: false,
    italic: false,
    locked: true,
    fixedSize: true
},

linetoolthumbup: {
    color: 'rgba( 0, 128, 0, 1)'
},
linetoolthumbdown: {
    color: 'rgba( 255, 0, 0, 1)'
},
linetoolpricelabel: {
    color: 'rgba( 102, 123, 139, 1)',
    backgroundColor: 'rgba( 255, 255, 255, 0.7)',
    borderColor: 'rgba( 140, 140, 140, 1)',
    fontWeight: 'bold',
    fontsize: 11,
    font: 'Arial',
    transparency: 30
},
linetoolrectangle: {
    color: 'rgba( 21, 56, 153, 1)',
    fillBackground: true,
    backgroundColor: 'rgba( 21, 56, 153, 0.5)',
    linewidth: 1.0,
    snapTo45Degrees:true,
    transparency: 50
},
linetoolrotatedrectangle: {
    color: 'rgba( 152, 0, 255, 1)',
    fillBackground: true,
    backgroundColor: 'rgba( 142, 124, 195, 0.5)',
    transparency: 50,
    linewidth: 1.0,
    snapTo45Degrees:true
},
linetoolellipse: {
    color: 'rgba( 153, 153, 21, 1)',
    fillBackground: true,
    backgroundColor: 'rgba( 153, 153, 21, 0.5)',
    transparency: 50,
    linewidth: 1.0
},
linetoolarc: {
    color: 'rgba( 153, 153, 21, 1)',
    fillBackground: true,
    backgroundColor: 'rgba( 153, 153, 21, 0.5)',
    transparency: 50,
    linewidth: 1.0
},
linetoolprediction: {
    singleChartOnly: true,
    linecolor: 'rgba( 28, 115, 219, 1)',
    linewidth: 2.0,

    sourceBackColor: 'rgba( 241, 241, 241, 1)',
    sourceTextColor: 'rgba( 110, 110, 110, 1)',
    sourceStrokeColor: 'rgba( 110, 110, 110, 1)',

    targetStrokeColor:'rgba( 47, 168, 255, 1)',
    targetBackColor:'rgba( 11, 111, 222, 1)',
    targetTextColor: 'rgba( 255, 255, 255, 1)',

    successBackground: 'rgba( 54, 160, 42, 0.9)',
    successTextColor: 'rgba( 255, 255, 255, 1)',

    failureBackground: 'rgba( 231, 69, 69, 0.5)',
    failureTextColor: 'rgba( 255, 255, 255, 1)',

    intermediateBackColor: 'rgba( 234, 210, 137, 1)',
    intermediateTextColor: 'rgba( 109, 77, 34, 1)',

    transparency: 10,
    centersColor: 'rgba( 32, 32, 32, 1)'
},
linetooltriangle: {
    color: 'rgba( 153, 21, 21, 1)',
    fillBackground: true,
    backgroundColor: 'rgba( 153, 21, 21, 0.5)',
    transparency: 50,
    linewidth: 1.0
},
linetoolcallout: {
    color: 'rgba( 255, 255, 255, 1)',
    backgroundColor: 'rgba( 153, 21, 21, 0.5)',
    transparency: 50,
    linewidth: 2.0,
    fontsize: 12,
    font:'Verdana',
    text: 'Text',
    bordercolor: 'rgba( 153, 21, 21, 1)',
    bold: false,
    italic: false,
    wordWrap: false,
    wordWrapWidth: 400
},
linetoolparallelchannel: {
    linecolor: 'rgba( 119, 52, 153, 1)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    extendLeft: false,
    extendRight: false,
    fillBackground: true,
    backgroundColor: 'rgba( 180, 167, 214, 0.5)',
    transparency: 50,
    showMidline: false,
    midlinecolor: 'rgba( 119, 52, 153, 1)',
    midlinewidth: 1.0,
    midlinestyle: LINESTYLE_DASHED
},
linetoolelliottimpulse: {
    degree: 7,
    showWave:true,
    color: 'rgba( 61, 133, 198, 1)',
    linewidth: 1
},
linetoolelliotttriangle: {
    degree: 7,
    showWave:true,
    color: 'rgba( 255, 152, 0, 1)',
    linewidth: 1
},
linetoolelliotttriplecombo: {
    degree: 7,
    showWave:true,
    color: 'rgba( 106, 168, 79, 1)',
    linewidth: 1
},
linetoolelliottcorrection: {
    degree: 7,
    showWave:true,
    color: 'rgba( 61, 133, 198, 1)',
    linewidth: 1
},
linetoolelliottdoublecombo: {
    degree: 7,
    showWave:true,
    color: 'rgba( 106, 168, 79, 1)',
    linewidth: 1
},
linetoolbarspattern: {
    singleChartOnly: true,
    color:'rgba( 80, 145, 204, 1)',
    mode:BARS_MODE,
    mirrored:false,
    flipped:false
},
linetoolghostfeed: {
    singleChartOnly: true,
    averageHL: 20,
    variance: 50,
    candleStyle: {
        upColor: '#6ba583',
        downColor: '#d75442',
        drawWick: true,
        drawBorder: true,
        borderColor: '#378658',
        borderUpColor: '#225437',
        borderDownColor: '#5b1a13',
        wickColor: '#737375'
    },
    transparency: 50
},
linetoolpitchfork: {
    fillBackground:true,
    transparency:80,
    style:PITCHFORK_STYLE_ORIGINAL,
    median: {
        visible: true,
        color: 'rgba( 165, 0, 0, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID
    },
    level0-8: LEVELS_TYPE_C
},
linetoolpitchfan: {
    fillBackground:true,
    transparency:80,
    median: {
        visible: true,
        color: 'rgba( 165, 0, 0, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID
    },
    level0-8: LEVELS_TYPE_C
},
linetoolgannfan: {
    showLabels: true,
    font: 'Verdana',
    fillBackground:true,
    transparency:80,
    level1-9: LEVELS_TYPE_F
},
linetoolganncomplex: {
    fillBackground:false,
    arcsBackground: {
        fillBackground: true,
        transparency: 80
    },
    levels: [/* 6 LEVELS_TYPE_D */],
    fanlines: [/* 11 LEVELS_TYPE_E */],
    arcs: [/* 11 LEVELS_TYPE_E */]
},
linetoolgannsquare: {
    color: 'rgba( 21, 56, 153, 0.8)',
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    font: 'Verdana',
    showTopLabels:true,
    showBottomLabels:true,
    showLeftLabels:true,
    showRightLabels:true,
    fillHorzBackground:true,
    horzTransparency:80,
    fillVertBackground:true,
    vertTransparency:80,
    hlevel1-7: LEVELS_TYPE_B,
    vlevel1-7: LEVELS_TYPE_B
},
linetoolfibspeedresistancefan: {
    fillBackground:true,
    transparency:80,
    grid: {
        color: 'rgba( 128, 128, 128, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID,
        visible:true
    },
    linewidth: 1.0,
    linestyle: LINESTYLE_SOLID,
    font: 'Verdana',
    showTopLabels:true,
    showBottomLabels:true,
    showLeftLabels:true,
    showRightLabels:true,
    snapTo45Degrees:true,
    hlevel1-7: LEVELS_TYPE_B,
    vlevel1-7: LEVELS_TYPE_B
},
linetoolfibretracement: {
    showCoeffs: true,
    showPrices: true,
    font: 'Verdana',
    fillBackground:true,
    transparency:80,
    extendLines:false,
    horzLabelsAlign: 'left',
    vertLabelsAlign: 'middle',
    reverse:false,
    coeffsAsPercents:false,
    trendline: {
        visible: true,
        color: 'rgba( 128, 128, 128, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_DASHED
    },
    levelsStyle: {
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID
    },
    level1-24: LEVELS_TYPE_B
},
linetoolfibchannel: {
    showCoeffs: true,
    showPrices: true,
    font: 'Verdana',
    fillBackground:true,
    transparency:80,
    extendLeft:false,
    extendRight:false,
    horzLabelsAlign: 'left',
    vertLabelsAlign: 'middle',
    coeffsAsPercents:false,
    levelsStyle: {
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID
    },
    level1-24:  LEVELS_TYPE_B
},
linetoolprojection: {
    showCoeffs: true,
    font: 'Verdana',
    fillBackground:true,
    transparency:80,
    color1: 'rgba( 0, 128, 0, 0.2)',
    color2: 'rgba( 255, 0, 0, 0.2)',
    linewidth: 1.0,
    trendline: {
        visible: true,
        color: 'rgba( 128, 128, 128, 1)',
        linestyle: LINESTYLE_SOLID
    },
    level1: LEVELS_TYPE_C
},
linetool5pointspattern: {
    color: 'rgba( 204, 40, 149, 1)',
    textcolor: 'rgba( 255, 255, 255, 1)',
    fillBackground: true,
    backgroundColor: 'rgba( 204, 40, 149, 0.5)',
    font: 'Verdana',
    fontsize:12,
    bold:false,
    italic:false,
    transparency: 50,
    linewidth: 1.0
},
linetoolcypherpattern: {
    color: '#CC2895',
    textcolor: '#FFFFFF',
    fillBackground: true,
    backgroundColor: '#CC2895',
    font: 'Verdana',
    fontsize:12,
    bold:false,
    italic:false,
    transparency: 50,
    linewidth: 1.0
},
linetooltrianglepattern: {
    color: 'rgba( 149, 40, 255, 1)',
    textcolor: 'rgba( 255, 255, 255, 1)',
    fillBackground: true,
    backgroundColor: 'rgba( 149, 40, 204, 0.5)',
    font: 'Verdana',
    fontsize:12,
    bold:false,
    italic:false,
    transparency: 50,
    linewidth: 1.0
},
linetoolabcd: {
    color: 'rgba( 0, 155, 0, 1)',
    textcolor: 'rgba( 255, 255, 255, 1)',
    font: 'Verdana',
    fontsize:12,
    bold:false,
    italic:false,
    linewidth: 2.0
},
linetoolthreedrivers: {
    color: 'rgba( 149, 40, 255, 1)',
    textcolor: 'rgba( 255, 255, 255, 1)',
    fillBackground: true,
    backgroundColor: 'rgba( 149, 40, 204, 0.5)',
    font: 'Verdana',
    fontsize:12,
    bold:false,
    italic:false,
    transparency: 50,
    linewidth: 2.0
},
linetoolheadandshoulders: {
    color: 'rgba( 69, 104, 47, 1)',
    textcolor: 'rgba( 255, 255, 255, 1)',
    fillBackground: true,
    backgroundColor: 'rgba( 69, 168, 47, 0.5)',
    font: 'Verdana',
    fontsize:12,
    bold:false,
    italic:false,
    transparency: 50,
    linewidth: 2.0
},
linetoolfibwedge: {
    singleChartOnly: true,
    showCoeffs: true,
    font: 'Verdana',
    fillBackground:true,
    transparency:80,
    trendline: {
        visible: true,
        color: 'rgba( 128, 128, 128, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID
    },
    level1-11: LEVELS_TYPE_C
},
linetoolfibcircles: {
    showCoeffs: true,
    font: 'Verdana',
    fillBackground:true,
    transparency:80,
    snapTo45Degrees:true,
    coeffsAsPercents:false,
    trendline: {
        visible: true,
        color: 'rgba( 128, 128, 128, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_DASHED
    },
    level1-11: LEVELS_TYPE_C
},
linetoolfibspeedresistancearcs: {
    showCoeffs: true,
    font: 'Verdana',
    fillBackground:true,
    transparency:80,
    fullCircles: false,
    trendline: {
        visible: true,
        color: 'rgba( 128, 128, 128, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_DASHED
    },
    level1-11: LEVELS_TYPE_C
},
linetooltrendbasedfibextension: {
    showCoeffs: true,
    showPrices: true,
    font: 'Verdana',
    fillBackground:true,
    transparency:80,
    extendLines:false,
    horzLabelsAlign: 'left',
    vertLabelsAlign: 'middle',
    reverse:false,
    coeffsAsPercents:false,
    trendline: {
        visible: true,
        color: 'rgba( 128, 128, 128, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_DASHED
    },
    levelsStyle: {
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID
    },
    level1-24: LEVELS_TYPE_B
},
linetooltrendbasedfibtime: {
    showCoeffs: true,
    font: 'Verdana',
    fillBackground:true,
    transparency:80,
    horzLabelsAlign: 'right',
    vertLabelsAlign: 'bottom',
    trendline: {
        visible: true,
        color: 'rgba( 128, 128, 128, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_DASHED
    },
    level1-11: LEVELS_TYPE_C
},
linetoolschiffpitchfork: {
    fillBackground:true,
    transparency:80,
    style:PITCHFORK_STYLE_SCHIFF,
    median: {
        visible: true,
        color: 'rgba( 165, 0, 0, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID
    },
    level0-8: LEVELS_TYPE_C
},
linetoolschiffpitchfork2: {
    fillBackground:true,
    transparency:80,
    style:PITCHFORK_STYLE_SCHIFF2,
    median: {
        visible: true,
        color: 'rgba( 165, 0, 0, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID
    },
    level0-8: LEVELS_TYPE_C
},
linetoolinsidepitchfork: {
    fillBackground:true,
    transparency:80,
    style:PITCHFORK_STYLE_INSIDE,
    median: {
        visible: true,
        color: 'rgba( 165, 0, 0, 1)',
        linewidth: 1.0,
        linestyle: LINESTYLE_SOLID
    },
    level0-8: LEVELS_TYPE_C
},
linetoolvisibilities: {
    intervalsVisibilities: {
        seconds:true,
        secondsFrom:1,
        secondsTo:59,
        minutes:true,
        minutesFrom:1,
        minutesTo:59,
        hours:true,
        hoursFrom:1,
        hoursTo:24,
        days:true,
        daysFrom:1,
        daysTo:366,
        weeks:true,
        months:true
    }
}
```

### Constants

These constants are used in the default properties of drawings. You cannot use their names directly. Use their values instead.

```javascript
LINESTYLE_SOLID = 0;
LINESTYLE_DOTTED = 1;
LINESTYLE_DASHED = 2;
LINESTYLE_LARGE_DASHED = 3;

LINEEND_NORMAL = 0;
LINEEND_ARROW  = 1;
LINEEND_CIRCLE = 2;

BARS_MODE = 0;
LINE_MODE = 1;
OPENCLOSE_MODE = 2;
LINEOPEN_MODE = 3;
LINEHIGH_MODE = 4;
LINELOW_MODE = 5;
LINEHL2_MODE = 6;

PITCHFORK_STYLE_ORIGINAL = 0;
PITCHFORK_STYLE_SCHIFF = 1;
PITCHFORK_STYLE_SCHIFF2 = 2;
PITCHFORK_STYLE_INSIDE = 3;

LEVELS_TYPE_A = {
    color: color,
    visible: visible
};

LEVELS_TYPE_B = {
    coeff: coeff,
    color: color,
    visible: visible
};

LEVELS_TYPE_C = {
    coeff: coeff,
    color: color,
    visible: visible,
    linestyle: linestyle,
    linewidth: linewidth
};

LEVELS_TYPE_D = {
    color: color,
    width: width,
    visible: visible
};

LEVELS_TYPE_E = {
    color: color,
    visible: visible,
    width: width,
    x: x,
    y: y
};

LEVELS_TYPE_F = {
    coeff1: coeff1,
    coeff2: coeff2,
    color: color,
    visible: visible,
    linestyle: linestyle,
    linewidth: linewidth
};
```
