## <a name="OWASP API Security Top 10 2023"></a> OWASP API Security Top 10 2023

A edição 2023 do **OWASP API Security Top 10** apresenta os dez riscos mais frequentes e impactantes que afetam interfaces de programação de aplicações (APIs). Esses riscos foram selecionados a partir de dados reais de incidentes de segurança, contribuições de pesquisadores e feedback da comunidade de desenvolvedores e profissionais de segurança. [Slides para apresentação](https://www.figma.com/slides/gdAQeZ6V3edqqzjt7FlblI/OWASP-API?t=MULMMAdPopTHY3zo-6)




## <a name="metodologia"></a>Metodologia OWASP API Top 10 2023

* **Coleta de dados**: relatórios de bug bounty, programas de divulgação de vulnerabilidades, fontes de threat-intel e estatísticas de grandes plataformas em nuvem.  
* **Classificação**: cada vulnerabilidade é avaliada por prevalência, exploração em ambiente real e impacto potencial (financeiro, reputacional e de privacidade). 
* **Comparação com edições anteriores**: o ranking 2023 traz novos nomes (ex.: “Unrestricted Resource Consumption”) e reorganiza posições para refletir o cenário atual de abuso de APIs.


| Tipo | Descrição |
|---|---|
| **API1 - Broken Object Level Authorization (BOLA)** | **O que é:** falha de controle que permite trocar o ID do objeto para acessar dados de outro usuário.<br>**Exemplo:** `GET /users/12345/profile` trocado para `.../12346/profile` devolve o perfil de outra pessoa.<br>**Prevenção:** validar autorização por objeto no back-end, usar IDs opacos (UUID) e testes automatizados de autorização no CI/CD. |
| **API2 - Broken Authentication** | **O que é:** falhas em login ou tokens que possibilitam brute-force, credential stuffing ou uso de tokens expirados.<br>**Exemplo:** ausência de lockout permite infinitas tentativas de senha.<br>**Prevenção:** MFA, bloqueio progressivo, sessões curtas, adoção de padrões como OAuth 2.0/OIDC e auditoria de tentativas de login. |
| **API3 - Broken Object Property Level Authorization (BOPLA)** | **O que é:** exposição ou alteração de propriedades internas do objeto, como flag de administração.<br>**Exemplo:** resposta JSON inclui `recentLocation` e `isAdmin` para usuários comuns.<br>**Prevenção:** utilizar DTOs que exponham apenas campos permitidos e validar no servidor. |
| **API4 - Unrestricted Resource Consumption** | **O que é:** falta de limites de taxa ou tamanho de requisição que causa DoS ou custos excessivos.<br>**Exemplo:** parâmetro `pageSize=100000` esgota memória ou banda.<br>**Prevenção:** rate-limiting, paginação com teto, quotas e time-outs. |
| **API5 - Broken Function Level Authorization (BFLA)** | **O que é:** endpoints privilegiados acessíveis a perfis comuns por falha no controle de roles.<br>**Exemplo:** usuário “default” invoca `DELETE /admin/users/7`.<br>**Prevenção:** matriz de ACL centralizada, verificação de autorização por perfil e testes automatizados. |
| **API6 - Unrestricted Access to Sensitive Business Flows** | **O que é:** fluxos críticos (ex.: reset de senha) expostos sem limitação ou antifraude, permitindo automação maliciosa.<br>**Exemplo:** bot gera milhares de cupons de desconto via API de checkout.<br>**Prevenção:** CAPTCHA adaptativo, detecção de comportamento anômalo e verificação extra de identidade. |
| **API7 - Server-Side Request Forgery (SSRF)** | **O que é:** API aceita URLs arbitrárias e faz requisições internas, acessando redes privadas ou metadata cloud.<br>**Exemplo:** chamada forjada busca `http://169.254.169.254/latest/meta-data/` e obtém chaves IAM.<br>**Prevenção:** whitelist de domínios, bloquear IPs internos e validar esquema/host. |
| **API8 - Security Misconfiguration** | **O que é:** configurações inseguras em qualquer camada (headers, portas, debug habilitado).<br>**Exemplo:** endpoint `/actuator/env` exposto em produção revela variáveis sensíveis.<br>**Prevenção:** IaC segura, scanners automáticos, hardening de contêineres e patching contínuo. |
| **API9 - Improper Inventory Management** | **O que é:** falta de inventário de APIs, versões ou documentação gerando “shadow APIs”.<br>**Exemplo:** versão v1 obsoleta continua ativa e sem autenticação forte.<br>**Prevenção:** catálogo central (OpenAPI/Swagger), “sunset” de versões antigas, varredura de sub-domínios e endpoints. |
| **API10 - Unsafe Consumption of APIs** | **O que é:** confiança excessiva em dados ou dependências de APIs de terceiros.<br>**Exemplo:** serviço de câmbio comprometido injeta payload que causa deserialização remota na API consumidora.<br>**Prevenção:** política “trust nothing”, validação de esquemas, TLS mútuo e due-diligence de parceiros. |

