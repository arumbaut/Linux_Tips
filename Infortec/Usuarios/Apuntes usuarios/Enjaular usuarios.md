Editar el /etc/ssh/sshd_config

Añadir:

#WO0000001049614  
Match User ofuscacion_sftp  
      AuthorizedKeysFile /infa_shared/srcfiles/CDWH/stg/ofuscacion/.ssh/authorized_keys  
      ChrootDirectory /infa_shared/srcfiles/CDWH/stg/ofuscacion  
      ForceCommand internal-sftp -u 0133