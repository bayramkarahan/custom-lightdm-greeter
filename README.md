# custom-lightdm-greeter
lightdm-greter edit

# 1-öncelikle 
lightdm kurulu olması lazım...sudo apt install lightdm

# 2-  sudo nano /etc/lightdm/lightdm.conf  

[Seat:*]
#type=local
#pam-service=lightdm
greeter-session=by-greeter #kendi greeterım

# 3- 2. adımda kullanılan by-greeter dosyasının ayar dosyası oluşturulur font teme ayarının yapıldığı yer
      
sudo nano /etc/lightdm/by-greeter                                       

[Seat:*]
greeter-session=by-greeter #kapattığımda sonuç değişmedi..

[GTK]
gtk-theme-name=Adwaita
gtk-application-prefer-dark-theme=true
gtk-cursor-theme-name=Adwaita

# 4-
sudo nano /usr/share/lightdm/greeters/by-greeter.desktop dosyası oluşturulacak
[Desktop Entry]
Name=Etap LightDM Greeter
Comment=Python LightDM Greeter
Exec=xcalc
Type=Application
X-LightDM-Session-Type=x

exec kısmına hangi dosyayı yazarsanız /usr/bin/xcalc gibi bir ikili gui dosyayını çalıştırabiliriz..

# 5- servis dosysı oluşturlur ki vardır düzenleme yapılablir..
 
/lib/systemd/system/lightdm.service                                 

[Unit]
Description=Light Display Manager
Documentation=man:lightdm(1)
After=systemd-user-sessions.service

# replaces plymouth-quit since lightdm quits plymouth on its own
Conflicts=plymouth-quit.service
After=plymouth-quit.service

# lightdm takes responsibility for stopping plymouth, so if it fails
# for any reason, make sure plymouth still stops
OnFailure=plymouth-quit.service

[Service]
# temporary safety check until all DMs are converted to correct
# display-manager.service symlink handling
ExecStartPre=/bin/sh -c '[ "$(cat /etc/X11/default-display-manager 2>/dev/null)" = "/usr/sbin/lightdm" ]'
ExecStart=/usr/sbin/lightdm
Restart=always
BusName=org.freedesktop.DisplayManager





