###### IPMI commands to reset a password

1. Listar usuario BMC id's.    
```bash  
ipmitool user list 1  
ID  Name         Callin  Link Auth    IPMI Msg   Channel Priv Limit  
1                    true    false      false      NO ACCESS  
2   USERID           true    true       true       ADMINISTRATOR  
3                    true    false      false      NO ACCESS  
4                    true    false      false      NO ACCESS  
5                    true    false      false      NO ACCESS  
6                    true    false      false      NO ACCESS  
7                    true    false      false      NO ACCESS  
8                    true    false      false      NO ACCESS  
9                    true    false      false      NO ACCESS  
10                   true    false      false      NO ACCESS  
11                   true    false      false      NO ACCESS  
12                   true    false      false      NO ACCESS  
13                   true    false      false      NO ACCESS  
```     
  
2.  Reset password root (userid 2)

```bash  
ipmitool user set password <ID> <CONTRASEÑA>  
ipmitool user set password 2 'SuperSecreto!'  
Set User Password command successful (user 2)  
ipmitool user set password 2 password@123  
```

***  
###### IPMI commands para crear nuevos usuarios

1. Crear usuario  
```bash  
ipmitool user set name "user id" "username"**  
# Ejmplo usuario test  
ipmitool user set name 3 test  
```    
2.  Establecer contraseña para el usuario  
```bash  
ipmitool user set password 3 password@123  
```  
3.  Habilitar nuevo usuario  
```  
ipmitool user enable 3  
```  
4.  Establecer privilegios de Administrador  
```bash  
ipmitool channel setaccess channelNO. userID callin=on ipmi=on link=on privilege=4  
# Ejemplo  
ipmitool channel setaccess 1 3 callin=on ipmi=on link=on privilege=4  
```    

> *Privilege levels:*  
    -   0x1 - Callback  
    -   0x2 - User  
    -   0x3 - Operator  
    -   0x4 - Administrator  
    -   0x5 - OEM Proprietary  
    -   0xF - No Access