---
title: "DHTMLX Touch for WebSharper available"
categories: "dhtmlx,f#,websharper"
abstract: "Today we are happy to announce the release of a new extension for WebSharper: DHTMLX Touch. This library provides a complete set of UI widgets, alongside a number of tools to seamlessly connect them to application data, resulting in web applications with an impressive native feel."
identity: "2061,74893"
---
Today we are happy to announce the release of a new extension for WebSharper: <a href=http://dhtmlx.com/touch/>DHTMLX Touch</a>.

"DHTMLX Touch is an HTML5-based JavaScript library for building mobile web applications. It’s not just a set of UI widgets, but a complete framework that allows you to create eye-catching, cross-platform web applications for mobile and touch-screen devices."

DHTMLX Touch is typically used as a full-page framework. It provides a lot of tools to ease the interaction between the application data, the display elements and the user, resulting in web applications with an impressive native feel.

The following sample displays a bar chart of the provided data alongside a table. It shows different ways to connect UI elements with application data.

```fsharp
type SalesData = { sales : float
                   year : int }

[<JavaScript>]
let salesData = [| {sales = 9.5; year = 2006}
                   {sales = 8.6; year = 2007}
                   {sales = 6.7; year = 2008}
                   {sales = 4.9; year = 2009}
                   {sales = 6.2; year = 2010} |]

[<JavaScript>]
let SimplePage () = 
    Div []
    |>! OnAfterRender (fun _ ->
        let chart = Chart(Type = ChartType.Bar,
                          Value = "#sales#",
                          Label = "#year#",
                          Data = salesData)
        let yearField = GridField(Id = "Year", Template = fun data ->
            let data = As<SalesData> data
            let d = Div [Text (string data.year)]
            d.Html)
        let figureField = GridField(Id = "Sales", Template = fun data ->
            let data = As<SalesData> data
            let d = Div [Text (string data.sales)]
            d.Html)
        let grid = Grid(Fields = [|yearField; figureField|],
                        Data = salesData)
        let page = Layout(Cols = [|chart; grid|])
        Dhx.Ui(page)
    )
```

<a href=http://websharper.com/samples/DHTMLXTouch>This more complete sample</a> shows how to build a simple shopping cart using DHTMLX Touch. It demonstrates user interaction and interaction with application data; it is reimplemented from [a sample](http://www.dhtmlx.com/blog/?p=968) by the DHTMLX authors.
You can download the WebSharper extension for DHTMLX Touch at <a href=http://www.websharper.com/extension/358-dhtmlx/None>this address</a>.
