#!/bin/bash

# Fájl elérési útja, amiben a figyelni kívánt felhasználók loginnevei vannak
USERS_FILE="/elérési/út/a/fájlra"

# Függvény a pop-up ablak megjelenítéséhez
show_popup() {
    zenity --info --text="$1"
}

# Függvény a bejelentkezés ellenőrzéséhez és pop-up ablak megjelenítéséhez
check_login() {
    while read -r line; do
        user=$(echo "$line" | awk '{print $1}')
        login_file="/var/run/utmp"

        if who | grep -wq "$user" && grep -wq "$user" "$USERS_FILE"; then
            login_info=$(last -1 -R -F "$login_file" | awk -v user="$user" '{if ($1 == user) print $1, $3, $4}')
            show_popup "$user logged in on $(hostname) at $login_info"
        fi
    done < "$USERS_FILE"
}

# Inotifywait segítségével figyeljük a USERS_FILE változásait
while inotifywait -e modify "$USERS_FILE"; do
    check_login
done

