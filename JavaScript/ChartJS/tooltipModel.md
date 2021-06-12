The `tooltipModel` contains parameters that can be used to render the tooltip. This is its structure:
```js
{
    // The items that we are rendering in the tooltip. See Tooltip Item Interface section
    dataPoints: TooltipItem[],

    // Positioning
    xPadding: number,
    yPadding: number,
    xAlign: string,
    yAlign: string,

    // X and Y properties are the top left of the tooltip
    x: number,
    y: number,
    width: number,
    height: number,
    // Where the tooltip points to
    caretX: number,
    caretY: number,

    // Body
    // The body lines that need to be rendered
    // Each object contains 3 parameters
    // before: string[] // lines of text before the line with the color square
    // lines: string[], // lines of text to render as the main item with color square
    // after: string[], // lines of text to render after the main lines
    body: object[],
    // lines of text that appear after the title but before the body
    beforeBody: string[],
    // line of text that appear after the body and before the footer
    afterBody: string[],
    bodyFontColor: Color,
    _bodyFontFamily: string,
    _bodyFontStyle: string,
    _bodyAlign: string,
    bodyFontSize: number,
    bodySpacing: number,

    // Title
    // lines of text that form the title
    title: string[],
    titleFontColor: Color,
    _titleFontFamily: string,
    _titleFontStyle: string,
    titleFontSize: number,
    _titleAlign: string,
    titleSpacing: number,
    titleMarginBottom: number,

    // Footer
    // lines of text that form the footer
    footer: string[],
    footerFontColor: Color,
    _footerFontFamily: string,
    _footerFontStyle: string,
    footerFontSize: number,
    _footerAlign: string,
    footerSpacing: number,
    footerMarginTop: number,

    // Appearance
    caretSize: number,
    caretPadding: number,
    cornerRadius: number,
    backgroundColor: Color,

    // colors to render for each item in body[]. This is the color of the squares in the tooltip
    labelColors: Color[],
    labelTextColors: Color[],

    // 0 opacity is a hidden tooltip
    opacity: number,
    legendColorBackground: Color,
    displayColors: boolean,
    borderColor: Color,
    borderWidth: number
}
```

### Properties
#### dataPoints (array)
An array of [[tooltipItem]] objects.
