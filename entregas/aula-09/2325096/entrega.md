# Entrega — Aula 09: Docker Registry e IA no CI/CD

**Aluno:** Eloísa Brandão  
**RA:** 2325096  
**Data:** 2026-10-18

## Repositório

- **Projeto:** [unifaat-devops-portfolio](https://github.com/brandelas/unifaat-devops-portfolio)
- **Implementação da Aula 09:** [aula-09](https://github.com/brandelas/unifaat-devops-portfolio/tree/main/aula-09)
- **Branch de desenvolvimento local:** `feature/aula-09-ecr-ai-review`

## 1. Validações locais

- [x] Testes automatizados da API: 9 testes passaram.
- [x] Imagem Docker `technova-api:aula09` construída localmente.
- [x] Contêiner executado localmente, com a porta 3001 do host mapeada para a porta 3000 do contêiner.
- [x] Endpoint `/health` respondeu com status `ok`.
- [x] Endpoint `/api/orders` retornou os pedidos de exemplo.
- [x] Testes automatizados dos scripts: 12 testes passaram.
- [x] Teste do Security Scan com um diff contendo padrões de risco identificou achados CRITICAL e retornou código de saída 1.
- [x] Teste do Security Scan com um diff sem os padrões analisados terminou sem achados.

Esses resultados correspondem a verificações locais. Eles não comprovam a execução dos workflows no GitHub Actions nem a publicação de imagens no Amazon ECR.

## 2. Amazon ECR

O projeto contém o script `aula-09/ecr-push.sh` e o workflow manual `build-push-ecr.yml`. A implementação prevê autenticação pelo AWS CLI e publicação de imagens com tags baseadas no commit, `latest` e tags semânticas quando houver uma tag Git no formato `vX.Y.Z`.

**Status: pendente de validação no AWS Academy Learner Lab, região `us-east-1`.**

- [ ] Executar o script e confirmar a publicação das imagens.
- [ ] Confirmar a criação ou existência do repositório `technova-api`.
- [ ] Fazer pull e executar uma imagem publicada.
- [ ] Verificar o procedimento de rollback por tag ou digest.
- [ ] Registrar evidências reais da publicação.

A publicação no ECR não foi realizada durante as validações locais documentadas nesta entrega.

## 3. Revisão automatizada no CI/CD

O projeto contém o workflow `ai-review.yml` e scripts para coletar e analisar o diff de pull requests.

A análise utiliza regras heurísticas para identificar possíveis credenciais expostas, uso de `eval()`, padrões de injeção, logs de depuração e blocos `catch` vazios. A implementação prevê que achados `CRITICAL` reprovem o quality gate, enquanto achados `WARNING` e `INFO`, isoladamente, não o reprovam.

A solução é baseada em regras locais e não chama um modelo de linguagem externo. Os resultados são indicativos e precisam de revisão humana; falsos positivos e falsos negativos são possíveis.

**Status: scripts e testes locais validados; execução real no GitHub Actions pendente.**

- [ ] Executar o workflow em um pull request.
- [ ] Verificar o comentário automático com os achados.
- [ ] Verificar o bloqueio do quality gate diante de um achado `CRITICAL`.
- [ ] Registrar o link da execução e capturas de tela reais.

A publicação de comentários em pull requests pode depender das permissões do repositório e da origem do pull request, especialmente quando ele vem de um fork.

## 4. Limitações

A API reutilizada da Aula 08 mantém os pedidos em memória. Os dados de exemplo não são persistidos em banco de dados e podem ser reinicializados quando o processo é reiniciado.

Os testes locais não substituem a validação integrada dos workflows no GitHub Actions nem os testes de publicação e rollback no ECR. A revisão automatizada não garante ausência de vulnerabilidades.

## 5. Evidências e pendências

- **Testes locais:** executados conforme os resultados descritos nesta entrega.
- **Docker local:** imagem construída e endpoints verificados.
- **GitHub Actions:** execução real ainda não realizada.
- **Amazon ECR:** publicação e rollback ainda não realizados.
- **Capturas de tela e links de execução:** serão adicionados após as validações correspondentes.

As etapas externas permanecem pendentes para não registrar como concluídas ações que ainda não foram executadas.
