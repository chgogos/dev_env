---
title: Εγκατάσταση και ρυθμίσεις προγραμματιστικού περιβάλλοντος ανάπτυξης εφαρμογών για C και C++ σε Windows
author: Χρήστος Γκόγκος
...

# Χρήση του Mingw-w64 στο VS Code

Οι παρακάτω οδηγίες αφορούν τη ρύθμιση του Visual Studio Code στα Windows έτσι ώστε να χρησιμοποιείται ο GCC C++ compiler και ο GDB debugger με σκοπό τη δημιουργία προγραμμάτων που τρέχουν σε Windows.

## Προαπαιτούμενα

- Εγκατάσταση του [Visual Studio Code](https://code.visualstudio.com/download)
- Εγκατάσταση της επέκτασης [C++ extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
- Εγκατάσταση της επέκτασης [Code Runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner)
- Εγκατάσταση του [mingw-w64](http://www.mingw-w64.org/) σε έναν κατάλογο που δεν έχει κενά στη διαδρομή του (όχι στην προκαθορισμένη διαδρομή του C:\\Program Files\\). Για παράδειγμα μπορεί να εγκατασταθεί στο C:\\Mingw-w64.
- Προσθήκη της διαδομής του καταλόγου bin του Mingw-w64 (π.χ. του c:\\Mingw-w64\\x86_64-8.1.0-win32-seh-rt_v6-rev0\\mingw64\\bin) στη μεταβλητή περιβάλλοντος (environment variable) PATH των Windows.

## Δημιουργία ενός χώρου εργασίας (workspace)

- Σε ένα νέο παράθυρο γραμμής εντολών, δημιουργήστε έναν άδειο κατάλογο projects στον οποίο θα τοποθετηθούν όλα τα VS Code projects. Στη συνέχεια δημιουργήστε έναν υποκατάλογο με όνομα helloworld. Οι εντολές που θα χρειαστεί να δοθούν είναι οι ακόλουθες:
    
    ```
    mkdir projects
    cd projects
    mkdir helloworld
    cd helloworld
    ```

- Ανοίξτε τον φάκελο helloworld μέσα από το VS Code (το VS Code έχει τη δυνατότητα εκτός από αρχεία να ανοίγει και καταλόγους οπότε απεικονίζει όλα τα αρχεία και τους υποκαταλόγους που περιέχονται).
- Όταν ολοκληρωθούν οι ρυθμίσεις θα υπάρχουν 3 αρχεία σε έναν υποκατάλογο .vscode που θα έχει δημιουργηθεί κάτω από τον τρέχοντα κατάλογο:
  - c_cpp_properties.json (διαδρομή μεταγλωττιστή και ρυθμίσεις IntelliSense)
  - tasks.json (ρυθμίσεις μεταγλώττισης και σύνδεσης)
  - launch.json (ρυθμίσεις debugger)

## Ρυθμίσεις σχετικά με τον compiler (c_cpp_properties.json)

* Πιέστε το συνδυασμό πλήκτρων Ctrl+Shift+P έτσι ώστε να ανοίξει η Command Palette (εναλλακτικά μπορεί να χρησιμοποιηθεί το μενού View > Command Palette). Πληκτρολογώντας C/C++ εμφανίζεται λίστα από την οποία θα πρέπει να επιλεγεί το C/C++: Edit Configurations (UI). Με αυτό τον τρόπο ανοίγει η σελίδα C/C++ IntelliSense Configurations και ότι αλλαγές γίνονται εκεί καταγράφονται στο αρχείο c_cpp_properties.json μέσα στον κατάλογο .vscode.
* Εύρεση της ρύθμισης compiler path. Το VS Code επιχειρεί να συμπληρώσει το πεδίο με τον προκαθορισμένο compiler που βρίσκει στο σύστημα. Προκειμένου να οριστεί ότι ο compiler είναι του Mingw-w64 θα πρέπει να περιέχει κάτι όπως: C:\\mingw-w64\\x86_64-8.1.0-win32-seh-rt_v6-rev0\\mingw64\\bin\\gcc.exe
* Εύρεση της ρύθμισης IntelliSense mode και ορισμός της ρύθμισης σε gcc-x64.
* Η τελική μορφή του c_cpp_properties.json θα πρέπει να είναι αντίστοιχη με την ακόλουθη:

    ```json
    {
    "configurations": [
        {
            "name": "Win64",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": 
            "C:\\mingw-w64\\x86_64-8.1.0-posix-seh-rt_v6-rev0\\mingw64\\bin\\gcc.exe",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64",
            "compilerArgs": []
        }
    ],
    "version": 4
}
    ```

## Ρυθμίσεις μεταγλώττισης και σύνδεσης (tasks.json)

* Πιέστε το συνδυασμό πλήκτρων Ctrl+Shift+P έτσι ώστε να ανοίξει η Command Palette. Πληκτρολογήστε task και επιλέξτε Tasks: Configure Default Build Task και στη συνέχεια επιλέξτε Create tasks.json from template και στη συνέχεια επιλέξτε Others.
* Αντικαταστήστε τα περιεχόμενα του tasks.json με 
  
    ```json
    {
        "version": "2.0.0",
        "tasks": [
            {
                "label": "build hello world",
                "type": "shell",
                "command": "gcc",
                "args": ["-g","-o","helloworld","helloworld.c"],
                "group": {
                    "kind": "build",
                    "isDefault": true
                }
            }
        ]
    }
    ```

* Στο αρχείο tasks.json το πεδίο command καθορίζει το πρόγραμμα που θα εκτελείται και το οποίο στη συγκεκριμένη περίπτωση είναι το gcc. Το πεδίο args μέσω ενός διανύσματος καθορίζει τα ορίσματα που θα περάσουν στο gcc και τα οποία θα πρέπει να είναι με τη σειρά που αναμένονται από το gcc. Το πεδίο isDefault μέσα στο πεδίο group εφόσον λάβει την τιμή true θα καθορίζει ότι το συγκεκριμένο task θα εκτελεστεί όταν θα πιεστεί ο συνδυασμός πλήκτρων Ctrl+Shift+B.  

## Ρυθμίσεις εκσφαλμάτωσης (launch.json)

* Πιέστε το συνδυασμό πλήκτρων Ctrl+Shift+P έτσι ώστε να ανοίξει η Command Palette. Πληκτρολογήστε launch και επιλέξτε Debug: Open launch.json και στη συνέχεια επιλέξτε GDB/LLDB.
* Στο αρχείο launch.json που δημιουργείται για το πεδίο program χρησιμοποιήστε το όνομα του προγράμματος helloworld.exe. Στο πεδίο miDebuggerPath αναθέστε τιμή που να ταιριάζει με τη διαδρομή για την εγκατάσταση του Mingw-w64.
* Η τελική μορφή του launch.json θα πρέπει να είναι αντίστοιχη με την ακόλουθη:

    ```json
    {
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "gcc.exe build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": true,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": 
            "C:\\mingw-w64\\x86_64-8.1.0-posix-seh-rt_v6-rev0\\mingw64\\bin\\gdb.exe",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "gcc.exe build active file"
        }
    ]
    }
    ```

## Προσθήκη πηγαίου κώδικα

* Στο VS Code επιλέξτε το μενού File > New File και ονομάστε το αρχείο helloworld.c
* Επικολλήστε τον ακόλουθο πηγαίο κώδικα:
  
    ```c
    #include <stdio.h>

    int main()
    {
        printf("hello, world\n");
        return 0;
    }
    ```
* Αποθηκεύστε (Ctrl+S), μεταγλωττίστε (Ctrl+Shift+B) και εκτελέστε εισάγοντας στο terminal την εντολή:

    ```console
    .\helloworld.exe
    ```

## Άλλα παραδείγματα

* Για ένα νέο project αρκεί να αντιγραφούν τα περιεχόμενα του καταλόγου .vscode και να γίνουν οι αλλαγές ονόματος πηγαίου κώδικα και ονόματος εκτελέσιμου.

# Αναφορές

* Using Mingw-w64 in VS Code: https://code.visualstudio.com/docs/cpp/config-mingw
* How to set the path and environment variables in Windows: https://www.computerhope.com/issues/ch000549.htm