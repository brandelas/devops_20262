# Entrega — Aula 09: Docker Registry e IA no CI/CD

**Aluno:** Eloísa Brandão  
**RA:** 2325096  
**Data:** 2026-10-22

## Repositório

- **Projeto:** [unifaat-devops-portfolio](https://github.com/brandelas/unifaat-devops-portfolio)
- **Implementação da Aula 09:** [aula-09](https://github.com/brandelas/unifaat-devops-portfolio/tree/feature/aula-09-ecr-ai-review/aula-09)
- **Branch de desenvolvimento local:** `feature/aula-09-ecr-ai-review`
- **Commit da aplicação publicada:** `f5121b9`
- **Tag Git:** [`v1.0.0`](https://github.com/brandelas/unifaat-devops-portfolio/tree/v1.0.0)

## 1. Validações locais

- [x] Testes automatizados da API: 9 testes passaram.
- [x] Imagem Docker `technova-api:aula09` construída localmente.
- [x] Contêiner executado localmente, com a porta 3001 do host mapeada para a porta 3000 do contêiner.
- [x] Endpoint `/health` respondeu com status `ok`.
- [x] Endpoint `/api/orders` retornou os pedidos de exemplo.
- [x] AI Review local: cinco padrões obrigatórios detectados, severidades CRITICAL/WARNING verificadas e diff limpo sem achados.
- [x] Proteção do `ecr-push.sh`: recusa publicar quando a árvore Git está suja.

Esses resultados locais não substituem a validação dos workflows em Pull Requests no GitHub Actions.

## 2. Amazon ECR

O projeto contém o script `aula-09/ecr-push.sh` e o workflow manual `build-push-ecr.yml`. A implementação prevê autenticação pelo AWS CLI e publicação de imagens com tags baseadas no commit, `latest` e tags semânticas quando houver uma tag Git no formato `vX.Y.Z`.

**Status: publicado e validado no AWS Academy Learner Lab, região `us-east-1`.**

- [x] Executar `ecr-push.sh` e confirmar a publicação das imagens.
- [x] Criar e confirmar o repositório `technova-api`.
- [x] Publicar tags `sha-f5121b9`, `latest`, `1.0.0`, `1.0` e `1`.
- [x] Fazer pull e executar imagens pelas tags SHA e `1.0.0`; `/health` respondeu `ok` e `/api/orders` retornou três pedidos.
- [x] Confirmar que as tags da release apontam para o digest `sha256:0a6729d3aa5136c3521a578d7e385b648fcc5fad88d1004bfecbe8f188e4f1ac`.

O pull de uma tag SHA anterior também foi executado, validando a recuperação de uma versão específica para rollback.

## 3. Revisão automatizada no CI/CD

O projeto contém o workflow `ai-review.yml` e scripts para coletar e analisar o diff de pull requests.

A análise utiliza regras heurísticas para identificar possíveis credenciais expostas, uso de `eval()`, padrões de injeção, logs de depuração e blocos `catch` vazios. A implementação prevê que achados `CRITICAL` reprovem o quality gate, enquanto achados `WARNING` e `INFO`, isoladamente, não o reprovam.

A solução é baseada em regras locais e não chama um modelo de linguagem externo. Os resultados são indicativos e precisam de revisão humana; falsos positivos e falsos negativos são possíveis.

**Status: scripts e regras locais validados; execução em Pull Request pendente.**

- [ ] Executar o workflow em um pull request.
- [ ] Verificar o comentário automático com os achados.
- [ ] Verificar o bloqueio do quality gate diante de um achado `CRITICAL`.
- [ ] Registrar o link da execução e capturas de tela reais.

Nenhum Pull Request foi aberto. Por isso, o comentário automático e o quality gate ainda não foram executados em GitHub Actions.

O workflow opcional de publicação no ECR foi acionado pela tag Git `v1.0.0`, mas falhou ao carregar credenciais AWS porque os secrets não estão configurados no GitHub. A publicação local via credenciais temporárias do Learner Lab funcionou. [Execução do workflow](https://github.com/brandelas/unifaat-devops-portfolio/actions/runs/39875333733).

## 4. Limitações

A API reutilizada da Aula 08 mantém os pedidos em memória. Os dados de exemplo não são persistidos em banco de dados e podem ser reinicializados quando o processo é reiniciado.

Os testes locais não substituem a validação integrada dos workflows no GitHub Actions nem os testes de publicação e rollback no ECR. A revisão automatizada não garante ausência de vulnerabilidades.

## 5. Evidências e pendências

- **Testes locais:** executados conforme os resultados descritos nesta entrega.
- **Docker local:** imagem construída e endpoints verificados.
- **Amazon ECR:** publicação, tags e pull validados; screenshots do console ainda pendentes.
- **GitHub Actions — publicação ECR:** execução falhou por ausência de secrets AWS.
- **AI Review e quality gate em PR:** pendentes; nenhum PR foi aberto.
- **Capturas de tela:** pendentes; nenhuma evidência visual foi fabricada.

As pendências de GitHub Actions e screenshots permanecem explícitas para não registrar ações não executadas como concluídas.
