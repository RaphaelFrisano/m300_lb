# LB2 Dokumentation - SMB Shares mit Portfreigabe
<p align="left">
  <img width="400" src="./vagrant.png">
  <img width="260" src="./git.png">
</p>

_Erstellt von [Raphael Frisano](https://github.com/RaphaelFrisano) am 12.03.2021_

---
<br>

# Grafische Übersicht
```
+----------------------------------+
! PC - Privatnetz (192.168.1.25)   !                 
! Port: 42000                      !	
!                                  !	
!      +--------------------+      !
!      ! SMB Server         !      !       
!      ! Host: SMB_fri      !      !
!      ! IP: localhost      !      !
!      ! Port: 445          !      !
!      ! Nat: 42000         !      !
!      +--------------------+      !
!                                  !	
+----------------------------------+
```

! Ordnerstruktur Übersicht !

# Beschreibung
Der SMB Server wird automatisch mit der vorgegeben Ordnerstruktur erstellt. Eine Firewall auf dem Server regelt dann, dass auf den Server einen Zugriff über NAT über den Port 42000 möglich ist.<br>
Die SMB Shares können dann im Netzwerk über den Namen des Virtuellen Servers gefunden und hinzugefügt werden.

# Testing
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum.

# Genutze Quellen
[Formatierung der grafischen Übersicht](https://github.com/mc-b/M300/blob/master/vagrant/fwrp/README.md)<br>
[Genutze Box als Basis](https://app.vagrantup.com/thefuncrew/boxes/ubuntu-18.04)