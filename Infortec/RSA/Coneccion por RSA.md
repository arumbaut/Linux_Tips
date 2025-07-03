Buscamos los servidores fisicos de los ESXi con el siguiente comando

```
#rvtquery --host %
vmwp31pv.jazztel.com               Datacenter_Yecora     CLVC-VMWV30PW                  7J42J-#####-#####-#####-28XN4  VMware_ESXi_6.5.0_build-14320405  AOTLXPRVC600003
vmwp31yv.jazztel.com               Datacenter_Torrejon   CLVC-VMWC30YW                  L5232-#####-#####-#####-A1MQH  VMware_ESXi_6.5.0_build-14320405  AOTLXPRVC600004
vmwp32pv.jazztel.com               Datacenter_Yecora     CLVC-VMWV30PW                  7J42J-#####-#####-#####-28XN4  VMware_ESXi_6.5.0_build-14320405  AOTLXPRVC600003
vmwp32yv.jazztel.com               Datacenter_Torrejon   CLVC-VMWC30YW                  L5232-#####-#####-#####-A1MQH  VMware_ESXi_6.5.0_build-14320405  AOTLXPRVC600004
vmwp33pv.jazztel.com               Datacenter_Yecora     CLVC-VMWV30PW                  7J42J-#####-#####-#####-28XN4  VMware_ESXi_6.5.0_build-14320405  AOTLXPRVC600003

```

Hacemos un ping para identificar su ip 
```
 #ping aotlxprvin10293.jazztel.com
```

Normalmete para la coneccion RSA la nomenclatura es poner un -rsa despues del nombre del servidor
```
 #ping aotlxprvin10293-rsa
```

Y asi obtenemos el ip por RSA del servidor para conectarnos via web por la ip RSA
En muchos casos tendremos que acceder a ellos desde una PC de salto de windows

