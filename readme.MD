install nodejs for catalogue 
-disable old and enable and install
###########################################
dnf module disable nodejs -y
dnf module enable nodejs:20 -y
dnf install nodejs -y