## Airspace Fixer

AirspaceFixerDotNet is a small lightweight project, forked from the original [AirspaceFixer](https://github.com/chris84948/AirspaceFixer) project and is a drop-in replacement for the original. This project only contains a single control - `AirspacePanel` with a few upgrades and fixes applied to the original. The memory leaks have been removed and the scaling math has been updated to support Per-Monitor DPI Scaling. The projects have updated to use SDK-style projects with .NET 4.6.2 and .NET 8.0.

If you've ever had to host any WinForms forms inside a WPF project, you might know about the dreaded Airspace problem. Basically, the WinForms control _**must**_ always be on top. Even if you try to put a modal dialog on top of the WinForms control, this Airspace issue forces the WinForms control on top.

`AirspacePanel` aims to fix this problem by allowing you to host any content you like inside of it, and swapping out the content for a screenshot of the content when you need to place a modal dialog on top of it. This swap is done by using the dependency property `FixAirspace`. Here is an example:

    <Window x:Class="AirspaceFixerSample.MainWindow" 
        xmlns:asf="clr-namespace:AirspaceFixer;assembly=AirspaceFixerDotNet">

    <asf:AirspacePanel FixAirspace="{Binding FixAirspace}">
        <WebBrowser x:Name="Browser" />
    </asf:AirspacePanel>

When used on .NET Framework platforms the following will need to add this to the application `app.manifest` file. This will ensure that Per-Monitor DPI scaling is applied to your application for every version of Windows back to Windows 7 and the updated scaling math can function correctly. The `gdiScaling` setting must be set to false or the scaling math will be incorrect.

    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <!-- fallback for Windows 7 and 8 -->
            <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/pm</dpiAware>
            <!-- adding v1 as fallback would result in v2 not being applied to dialogs on capable systems -->
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
            <!-- disables GDI DPI scaling -->
            <gdiScaling xmlns="http://schemas.microsoft.com/SMI/2017/WindowsSettings">false</gdiScaling>
        </windowsSettings>
    </application>

See the sample app for more info on how to use it.

AirspaceFixer is also available in Nuget to include in your projects as a package.
