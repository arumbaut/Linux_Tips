Nota las interfaces fisicas son independientes y se le asignas interfaces como virtuales que van conectadas a los webservers 
Tener encuenta si es por fichero nmcli es mediante este software nmcli 

1. Identificar la interfaz

	ip a | grep X.X.X.X

2. Eliminar IP en caliente

	ip a del X.X.X.X/XX dev ethX:X

3. Renombrar los ficheros BAJA_icfg_ethX o BAJA_rule_ethX

4. Eliminar rule (si corresponde)

	ip rule del XXXXX