Cześć, mam podłaczone do sterownika moduły EtherCAT. Możesz sprawdzić czy są widoczne na sieci ? 

// ręcznie skanujemy urządzenia

Chciałbym wygenerować plik GVL_IO z zmiennymi do podłączenia pod peryferia. Zaproponujesz mi listę zmiennych na podstawie istniejącej konfiguracji ? Pamiętaj o tym żeby sprawdzić czy zmienna jest wejściem czy wyjściem

// zmiana na PLC Agenta

Zróbmy zatem blok funkcyjny GVL_IO z tymi zmiennymi. Pamiętaj aby używać AT %I* oraz AT %Q*

// Pamiętmy o opcji Always Link

Zmienne są już w instancji PLC do zmapowania. Podłącz je do poszczególnych modułów