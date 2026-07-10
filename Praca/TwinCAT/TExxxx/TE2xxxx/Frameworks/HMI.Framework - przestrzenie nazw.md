# HMI.Framework - przestrzenie nazw

Zrodlo: `Packages/Beckhoff.TwinCAT.HMI.Framework.14.4.70/runtimes/native1.12-tchmi/dist/index/TcHmi.d.ts`.

Pakiet `Beckhoff.TwinCAT.HMI.Framework` udostepnia glowna globalna przestrzen `TcHmi` oraz podprzestrzenie dla API frameworka, kontrolek systemowych, serwera, symboli, tematow i UI.

## Glowne przestrzenie `TcHmi`

- `TcHmi`
- `TcHmi.Access`
- `TcHmi.Animation`
- `TcHmi.AnimationProvider`
- `TcHmi.Base64BinaryReader`
- `TcHmi.Base64BinaryWriter`
- `TcHmi.Binding`
- `TcHmi.Callback`
- `TcHmi.Config`
- `TcHmi.ControlFactory`
- `TcHmi.Controls`
- `TcHmi.Controls.System`
- `TcHmi.DialogManager`
- `TcHmi.Engineering`
- `TcHmi.Engineering.ErrorPane`
- `TcHmi.Environment`
- `TcHmi.Errors`
- `TcHmi.EventProvider`
- `TcHmi.Exception`
- `TcHmi.FileUploader`
- `TcHmi.FilterInstance`
- `TcHmi.Function`
- `TcHmi.Functions`
- `TcHmi.IFunction`
- `TcHmi.Interval`
- `TcHmi.Keyboard`
- `TcHmi.Locale`
- `TcHmi.Localization`
- `TcHmi.LocalStorage`
- `TcHmi.Log`
- `TcHmi.Log.Controls`
- `TcHmi.ObjectResolver`
- `TcHmi.Server`
- `TcHmi.StyleProvider`
- `TcHmi.Symbol`
- `TcHmi.SymbolExpression`
- `TcHmi.System`
- `TcHmi.TcSpeech`
- `TcHmi.Theme`
- `TcHmi.TopMostLayer`
- `TcHmi.Trigger`
- `TcHmi.Type`
- `TcHmi.UiProvider`
- `TcHmi.ValueConverter`
- `TcHmi.View`

## Kontrolki systemowe

- `TcHmi.Controls.System.TcHmiControl`
- `TcHmi.Controls.System.TcHmiFlexContainer`
- `TcHmi.Controls.System.TcHmiGrid`
- `TcHmi.Controls.System.TcHmiGridContainer`
- `TcHmi.Controls.System.TcHmiPopup`
- `TcHmi.Controls.System.TcHmiRegion`
- `TcHmi.Controls.System.TcHmiUserControlHost`

## Serwer

- `TcHmi.Server.ADS`
- `TcHmi.Server.AuditTrail`
- `TcHmi.Server.AuditTrail.CreateAuditLogEntry`
- `TcHmi.Server.Domains`
- `TcHmi.Server.Events`
- `TcHmi.Server.Historize`
- `TcHmi.Server.RecipeManagement`
- `TcHmi.Server.UserManagement`

## Pozostale podprzestrzenie zagniezdzone

- `TcHmi.Symbol.ObjectResolver`
- `TcHmi.TcSpeech.AudioEntityPriority`
- `TcHmi.Theme.Properties`
- `TcHmi.Theme.Resources`
- `TcHmi.Trigger.Actions`
- `TcHmi.Type.Schema`
- `TcHmi.UiProvider.PopupProvider`
- `TcHmi.UiProvider.PopupProvider.HtmlElementBox`
- `TcHmi.UiProvider.PopupProvider.InputPrompt`
- `TcHmi.UiProvider.PopupProvider.InteractiveWritePrompt`
- `TcHmi.UiProvider.PopupProvider.MessageBox`
- `TcHmi.UiProvider.PopupProvider.Popup`

## Jak to czytac

- `TcHmi.*` to glowne API frameworka uruchamiane po stronie klienta HMI.
- `TcHmi.Controls.System.*` obejmuje bazowe/systemowe kontrolki i kontenery.
- `TcHmi.Server.*` grupuje typy i API zwiazane z komunikacja z serwerem HMI, ADS, eventami, historiami, recepturami i uzytkownikami.
- `TcHmi.UiProvider.PopupProvider.*` opisuje typy popupow i okien dialogowych.
- `TcHmi.Theme.*`, `TcHmi.Localization`, `TcHmi.Locale` dotycza wygladu oraz lokalizacji.

