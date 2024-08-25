# Fluxo de Desenvolvimento

Este fluxo de desenvolvimento descreve como as branches devem ser gerenciadas no repositório, desde o início do trabalho em uma tarefa até a conclusão e a integração na branch principal.

## 1. Atualizar a Branch `main`
- Antes de iniciar o trabalho em uma nova tarefa, certifique-se de estar na branch `main`.
- Atualize a branch main para garantir que você esteja trabalhando na versão mais recente:
```sql
git checkout main
git pull
```

## 2. Criar uma Nova Branch para a Tarefa
- Toda nova tarefa deve ser desenvolvida em uma branch separada, criada a partir da branch `main`.
- Para criar a branch, use o comando:
```sql
git checkout -b <numero_da_tarefa>
```
- Substitua `<numero_da_tarefa>` pelo número da tarefa. Por exemplo, para a tarefa `112922`:
```sql
git checkout -b 112922
```

## 3. Desenvolver a Tarefa
- Realize o desenvolvimento e faça commits frequentemente, seguindo as boas práticas de commit.
- Use os seguintes comandos para adicionar suas alterações ao controle de versão:
```sql
git add .
git commit -m "Mensagem do commit"
git push origin <numero_da_tarefa>
```
- Certifique-se de que todos os testes estão passando antes de prosseguir para o próximo passo.
- **Padrão de Mensagem de Commit**: Utilize uma mensagem clara e descritiva para o commit. O formato recomendado é:
```sql
#numero_da_tarefa tipo: breve descrição
```
- **Exemplo:** `#112922 feat: Implementação da lógica de sincronização de músicas`
- Os tipos de commits podem incluir:
  - `feat`: nova funcionalidade
  - `fix`: correção de bugs

## 4. Criar um Pull Request (PR)
- Após subir as alterações para o GitHub, siga os passos abaixo para criar um Pull Request (PR):
  - Acesse o repositório no GitHub.
  - Clique na aba `Pull requests`.
  - Clique no botão `New pull request`.
  - Selecione a branch `develop` como base (`base`) e a sua branch de tarefa como compare (`compare`).
  - Verifique as alterações e, se tudo estiver correto, clique em `Create pull request`.
  - No campo de descrição, informe detalhadamente as alterações realizadas e faça referência ao número da tarefa.
  - Clique em `Create pull request` novamente para confirmar.

## 5. Atualizar a Branch `main` para Iniciar uma Nova Tarefa
- Após criar o PR, volte para a branch `main`, atualize ela para garantir que você tenha a versão mais recente.para iniciar a próxima tarefa:
```sql
git checkout main
git pull
```
- Repita todo o processo novamente (criar branch, desenvolver, etc.) para a próxima tarefa.

## 6. Revisão de Código
- O líder da equipe será responsável por revisar o código no PR que será enviado para a branch `develop`.
- Caso haja necessidade de ajustes, o líder deixará comentários no PR para que as correções sejam feitas.
- Após as correções, faça novos commits na mesma branch e atualize o PR.
- Após a aprovação do PR e a realização do merge na branch `develop`, o líder da equipe deverá criar um novo PR para atualizar a branch `main` através da branch `develop`.
- Esse processo garante que todas as alterações aprovadas e testadas sejam integradas na branch `main`, mantendo a integridade e a estabilidade do código principal.
