# aka-secret-manager


Desenvolvendo um Secret Manager para estudos de criptografia e API em .NET 10.

## Rascunho:

criptografia em repouso e em tránsito. <-- AES-256 e a geração de hash para garantir imutabilidade. Em trânsito, TLS 1.3 ou deixar o cliente gerar um padrão de uso, que vai ser compartilhaod no handshake. 

usuários acessam a aplicação e mantem sessão ativa via jwt ou nonce (prova de posse de uma private key). Além de user, podemos ter um sistema de crachas, como a MS possui para as aplicações via App Service. Uma aplicação pode ter uma chave privada e usamos ela via nonce para provar e autorizar ou não o envio de uma secret para o caller. Aproveitando a separação de usuário e sistema, daria para fazer um IAM simples para roles RBAC mesmo, pela simplicidade do sistema.

Versionamento configurável pelo user, para evitar quebrar e conseguir recuperar uma secret.

Uma feature a mais interessante poderia ser a de gerar secrets random com rotação automática e programada, pois vai que o user quer uma secret auto-rotacionável e não quer rotacionar na mão!

Auditoria completa de acessos, requestst e modificações, etc... <-- Via dashboard simples
