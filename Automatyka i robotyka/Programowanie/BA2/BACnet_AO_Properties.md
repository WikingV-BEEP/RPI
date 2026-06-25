---
tags:
  - analog-output
  - automatyka-i-robotyka
  - automatyka-i-robotyka/programowanie
  - automatyka-i-robotyka/programowanie/ba2
  - bacnet
  - beckhoff
  - plc
  - programowanie
  - wlasciwosci
---

## BACnet – Właściwości obiektu Analog Output (AO)

Obiekt typu **Analog Output (AO)** w protokole BACnet zawiera zestaw zdefiniowanych właściwości. Każda właściwość posiada:

- unikalny **identyfikator**,
- określony **typ danych**,
- kod zgodności (**CC**) oznaczający, czy jest wymagana, opcjonalna lub zapisywalna.

---

## Kody zgodności (Conformance Code – CC)

- `R` – Required (wymagana),
- `O` – Optional (opcjonalna),
- `W` – Writable (zapisywalna).

---

## Lista właściwości

| **Property Identifier**           | **Datatype**                                | **CC**   |
|----------------------------------|---------------------------------------------|----------|
| Object_Identifier                | BACnetObjectIdentifier                      | R        |
| Object_Name                      | CharacterString                             | R        |
| Object_Type                      | BACnetObjectType                            | R        |
| Present_Value                    | REAL                                        | W        |
| Description                      | CharacterString                             | O        |
| Device_Type                      | CharacterString                             | O        |
| Status_Flags                     | BACnetStatusFlags                           | R        |
| Event_State                      | BACnetEventState                            | R        |
| Reliability                      | BACnetReliability                           | O        |
| Out_Of_Service                   | BOOLEAN                                     | R        |
| Units                            | BACnetEngineeringUnits                      | R        |
| Min_Pres_Value                   | REAL                                        | O        |
| Max_Pres_Value                   | REAL                                        | O        |
| Resolution                       | REAL                                        | O        |
| Priority_Array                   | BACnetPriorityArray                         | R        |
| Relinquish_Default               | REAL                                        | R        |
| COV_Increment                    | REAL                                        | O1       |
| Time_Delay                       | Unsigned                                    | O2,4     |
| Notification_Class               | Unsigned                                    | O2,4     |
| High_Limit                       | REAL                                        | O2,4     |
| Low_Limit                        | REAL                                        | O2,4     |
| Deadband                         | REAL                                        | O2,4     |
| Limit_Enable                     | BACnetLimitEnable                           | O2,4     |
| Event_Enable                     | BACnetEventTransitionBits                   | O2,4     |
| Acked_Transitions                | BACnetEventTransitionBits                   | O2,4     |
| Notify_Type                      | BACnetNotifyType                            | O2,4     |
| Event_Time_Stamps                | BACnetARRAY[3] of BACnetTimeStamp           | O2,4     |
| Event_Message_Texts              | BACnetARRAY[3] of CharacterString           | O3,4     |
| Profile_Name                     | CharacterString                             | O        |
| Event_Message_Texts_Config       | BACnetARRAY[3] of CharacterString           | O4       |
| Event_Detection_Enable           | BOOLEAN                                     | O2,4     |
| Event_Algorithm_Inhibit_Ref      | BACnetObjectPropertyReference               | O4       |
| Event_Algorithm_Inhibit          | BOOLEAN                                     | O4,5     |
| Time_Delay_Normal                | Unsigned                                    | O4       |
| Reliability_Evaluation_Inhibit   | BOOLEAN                                     | O6       |
| Property_List                    | BACnetARRAY[N] of BACnetPropertyIdentifier  | R        |
