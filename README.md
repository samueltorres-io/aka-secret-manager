# aka-secret-manager


Desenvolvendo um Secret Manager para estudos de criptografia e API em .NET 10 + Rust.

## Rascunho:

criptografia em repouso e em tránsito. <-- AES-256 e a geração de hash para garantir imutabilidade. Em trânsito, TLS 1.3 ou deixar o cliente gerar um padrão de uso, que vai ser compartilhaod no handshake. 

usuários acessam a aplicação e mantem sessão ativa via jwt ou nonce (prova de posse de uma private key). Além de user, podemos ter um sistema de crachas, como a MS possui para as aplicações via App Service. Uma aplicação pode ter uma chave privada e usamos ela via nonce para provar e autorizar ou não o envio de uma secret para o caller. Aproveitando a separação de usuário e sistema, daria para fazer um IAM simples para roles RBAC mesmo, pela simplicidade do sistema.

Versionamento configurável pelo user, para evitar quebrar e conseguir recuperar uma secret.

Uma feature a mais interessante poderia ser a de gerar secrets random com rotação automática e programada, pois vai que o user quer uma secret auto-rotacionável e não quer rotacionar na mão!

Auditoria completa de acessos, requestst e modificações, etc... <-- Via dashboard simples

RUST para os processos críticos e que exigem segurança de memória e c# .net com o restante.
Banco vai ser o PostgreSQL para os dados binários BYTEA.
Redis para database cache de dados CRIPTOGRAFADOS. Nenhuma secret fica em plain text no cache e nenhuma info imprtante. Apenas um simples Database-Cache, já que o sistema vai ter muitas requests (na ideia da aruqitetura).

KEK e DEKs para a criptografia.

labels de vervionamento: CURRENT | PREVIOUS | PENDIND

Hash nos logs.

Arquitetura: Vertical Slice Architecture

Sistema seria separado em camadas

- CAMADA KMS: Gerencia a criptografia (cifra/decifra outras chaves).
- Secret Store: Gerencia o **ciclo de vida** do secret: armazenamento, controle de acesso, rotação, auditoria.
- Aplicação: Autentica-se, lê o secret em runtime, usa e descarta.
- IAM: Gerenciamento de user e crachás


Arquivo -> DEK -> KEK (KEK poderia ser gerada por projeto/uusário) e a dek é salva junto e descartada da memória. <-- rust

---

## Referencias

- https://zerotohero-dev.translate.goog/inbox/dek-kek/?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc
- https://dev.to/devdoido/conheca-o-clean-vertical-sliced-architecture-a-uniao-dos-2-mundos-4gnd
- https://github.com/Infisical/infisical
- https://dev.to/gramli/net-vertical-slice-architecture-vs-clean-architecture-a-practical-comparison-using-real-apis-4mck
- https://codewithmukesh.com/blog/api-key-authentication-aspnet-core/
- https://codewithmukesh.com/blog/minimal-apis-aspnet-core/
- https://hidekazu-konishi.com/entry/aws_kms_envelope_encryption_and_data_key_caching_guide.html
- https://notes.kodekloud.com/docs/HashiCorp-Certified-Vault-Associate-Certification/Learning-the-Vault-Architecture/Unsealing-with-Key-Shards/page
- https://developer.hashicorp.com/vault/docs/internals/security
