Cześć, mam podłączone do sterownika urządzenia. Przeskanuj sterownik.

// ręcznie skanujemy urządzenia

Chciałbym wygenerować plik GVL_IO z zmiennymi do podłączenia pod peryferia. Zaproponujesz mi listę zmiennych na podstawie istniejącej konfiguracji. Pamiętaj o tym żeby sprawdzić czy zmienna jest wejściem czy wyjściem

// zmiana na PLC Agenta

Zróbmy zatem GVL_IO z tymi zmiennymi. Pamiętaj aby używać AT %I* oraz AT %Q*

// Pamiętmy o opcji Always Link

Zmienne są już w instancji PLC gotowe do zmapowania. Podłącz je do poszczególnych modułów