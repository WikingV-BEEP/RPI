---
tags:
  - bacnet
  - protokoły_komunikacyjne
  - programowanie
  - plc
  - beckhoff
---

## BACnet – Obiekty

### Czym są obiekty?

- Obiektowa reprezentacja informacji urządzeń do komunikacji.
- Odwzorowania urządzeń i funkcji (np. czujników, aktuatorów, sterowników, timerów).
- Reprezentują funkcje urządzeń z perspektywy „świata zewnętrznego”.
- Każdy typ obiektu zawiera właściwości obowiązkowe i opcjonalne.

---

## Typy obiektów

### Obiekty danych (Data point objects)

- **Analog Input / Output / Value**  
- **Binary Input / Output / Value**  
- **Counter / Pulse**

### Obsługa alarmów

- **Notification class** – dystrybucja komunikatów alarmowych  
- **Event enrollment** – określenie warunków alarmowych  

### Różne

- Informacje o urządzeniu (Device Object)  
- Kalendarz / harmonogram (Scheduler / Calendar)  
- Logowanie trendów (Trend logging)  
- Pętla sterowania (Loop / PID)  
- Program / Plik (Program / File)

---

## Przykładowe obiekty i ich zastosowanie

| **OBJECT**         | **EXAMPLE OF USE**                                                          |
| ------------------ | --------------------------------------------------------------------------- |
| Analog Input       | Sensor input                                                                |
| Analog Output      | Control output                                                              |
| Analog Value       | Setpoint or other analog control system parameter                           |
| Binary Input       | Switch input                                                                |
| Binary Output      | Relay output                                                                |
| Binary Value       | Binary (digital) control system parameter                                   |
| Calendar           | Defines a list of dates, such as holidays or special events, for scheduling |
| Command            | Writes multiple values to multiple objects/devices (e.g., day/night mode)   |
| Device             | Shows device capabilities, vendor, firmware version, etc.                   |
| Event Enrollment   | Defines conditions for errors/alarms; can trigger Notification Class        |
| File               | Read/write access to device files                                           |
| Group              | Access to multiple object properties in one operation                       |
| Loop               | Standardized access to a control loop                                       |
| Multi-state Input  | Represents status (e.g., On, Off, Defrost cycles)                           |
| Multi-state Output | Desired state of multistate process (e.g., cooling phases)                  |
| Notification Class | Defines devices to be notified by alarms                                    |
| Program            | Allows a program to start, stop, load, etc., and read its status            |
| Schedule           | Defines weekly operations; integrates with Calendar                         |

