XPU - całość xplanar (Processing Unit)
Tile - to takie kwadratowe - podłoga
Mover - to takie latające - mover
Track - to ścieżka po której lata mover
Stacja - to punkt na ścieżce? gdzie np. mover sie zatrzymuje i realizuje proces jakiś. np nakładanie kleju. Orchestruje ruchem moverów (przeważnie)

Tile, Mover, Track - te rzeczy są dostępne w driverze
Stacja - abstrakcyjny twór

Liby:

Tc3_PlanarMotion - bazowy - do ruchów ptp i na ścieżce
Tc3_XPlanarStandard - rozszerzony - ułatwia ruchy, ma bloczek FIFO do implementowania stacji i parę innych. Wkrótce będzie wycofany
Tc3_XPlanarUtilites - bloczek do gadania z HW - TcCOM i IO


GVL_Application.ipMoverApplication
![[Pasted image 20261109140600.png]]

Mover - pozycja - ta sama co na PLC HMI?
Track - ?
Station - ma property - można użyć
Tile - Tc3_XPlanarUtilites - trzeba znaleść