Falhas de login do admin



action:login AND status:failed



Login falho por motivo (ex: senha inválida)



action:login AND status:failed AND reason:passwd\_invalid



Mudanças de configuração (Edit/Delete)



\_exists\_:cfgpath AND action:(Edit Delete)



Política apagada



cfgpath:"firewall.policy" AND action:Delete



Tentativas de login por usuário



action:login AND \_exists\_:user



Acessos SSH aceitos (tráfego)



service:SSH AND action:accept



Admin login falho (por IP de origem)



action:login AND status:failed AND \_exists\_:srcip



Admin login bem-sucedido



action:login AND status:success



Config changes do usuário “admin”



\_exists\_:cfgpath AND action:(Edit Delete) AND user:admin

