---

---
> [!warning] 
> Pour activer le service SSH, le switch doit avoir un mot de passe.

```plain text
Switch#show ip ssh
Switch#configure terminal
Switch(config)#enable secret 1234-MetroPole:1234
Switch(config)#ip domain-name metropolecg.com
Switch(config)#ip ssh version 2
Switch(config)#crypto key generate rsa
How many bits in tje modulus [512]:1024
Switch(config)#username admin secret 1234-MetroPole:1234
Switch(config)#line vty 0 15
Switch(config-line)#transport input ssh
Switch(config-line)#login local
Switch(config-line)#exit
```
