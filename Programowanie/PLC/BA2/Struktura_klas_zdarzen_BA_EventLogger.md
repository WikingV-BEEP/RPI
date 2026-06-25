---
tags:
  - programowanie/plc
  - programowanie/plc/ba2
  - alarmy
  - ba-view
  - beckhoff
  - eventlogger
  - plc
  - programowanie
  - zdarzenia
---

## Tworzenie alarmów dla EventLoggera w BA – `FB_BA_EventClassesAMEV2017`

W celu klasyfikacji alarmów i zdarzeń dla systemu EventLogger w środowisku Beckhoff TwinCAT 3, wykorzystuje się specjalny blok funkcyjny dziedziczący po `FB_BA_View`.

---

## Struktura bloku `FB_BA_EventClassesAMEV2017`

```iec
FUNCTION_BLOCK FB_BA_EventClassesAMEV2017 EXTENDS FB_BA_View
VAR_INPUT CONSTANT
    LifeSafety         : FB_BA_EC;
    SafetyMsg          : FB_BA_EC;
    AlarmMsg           : FB_BA_EC;
    FaultMsg           : FB_BA_EC;
    MaintenanceMsg01   : FB_BA_EC;
    MaintenanceMsg02   : FB_BA_EC;
    SystemMsg          : FB_BA_EC;
    ManualOp           : FB_BA_EC;
    OtherMsg           : FB_BA_EC;
END_VAR


```

Każde pole typu `FB_BA_EC` (Event Class) reprezentuje **kategorię alarmów lub zdarzeń**, które mogą być logowane i obsługiwane przez system EventLoggera.

---

## Opis pól (klasyfikacje alarmów)

|**Nazwa**|**Przeznaczenie**|
|---|---|
|`LifeSafety`|Alarmy zagrożenia życia (np. pożar, gaz)|
|`SafetyMsg`|Ogólne alarmy bezpieczeństwa|
|`AlarmMsg`|Standardowe alarmy|
|`FaultMsg`|Błędy urządzeń lub systemu|
|`MaintenanceMsg01`|Wiadomości serwisowe – grupa 1|
|`MaintenanceMsg02`|Wiadomości serwisowe – grupa 2|
|`SystemMsg`|Komunikaty systemowe|
|`ManualOp`|Alarmy operacji ręcznych|
|`OtherMsg`|Inne komunikaty (rezerwowe)|

---

## Zastosowanie

- Blok ten może być używany jako centralny punkt klasyfikacji alarmów.
    
- Każdy obiekt (np. silnik, czujnik, centrala) może być powiązany z jedną z tych klas.
    
- Ułatwia filtrowanie, kolorowanie, kategoryzowanie i raportowanie zdarzeń w HMI.
    

> [!note]  
> Blok `FB_BA_EventClassesAMEV2017` może być podłączony jako element struktury `FB_BA_View`, dzięki czemu pasuje do hierarchii DPAD i pozwala na logiczne przypisanie klas alarmów do określonych instalacji lub sekcji obiektu.

