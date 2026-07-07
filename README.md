# Exercícios Práticos Intermediários de Cypress - OrangeHRM

Este repositório contém uma bateria de exercícios práticos para aprimorar suas habilidades em testes E2E utilizando Cypress. Todos os exercícios são voltados para interações entre páginas do **OrangeHRM**.

## Pré-requisitos e Setup
- **URL Base:** `https://opensource-demo.orangehrmlive.com/web/index.php/auth/login`
- **Credenciais de Acesso (Login com Sucesso):** 
  - **Username:** `Admin`
  - **Password:** `admin123`

---

### Exercicio 1: Otimizando a Inicializacao com baseUrl
Objetivo: Configurar a URL globalmente para evitar o carregamento duplo e reloads desnecessarios no inicio dos testes.
1. Abra o arquivo `cypress.config.js` (ou `.ts`) na raiz do seu projeto.
2. Adicione a propriedade `baseUrl` definindo o valor como a URL Base fornecida.
3. Crie um arquivo de teste e substitua o uso de `cy.visit('https://...')` por `cy.visit('/')`.
4. Execute o teste e valide que o Cypress acessou corretamente a tela de login sem recarregar a pagina principal.

### Exercicio 2: Protecao de Credenciais com Variaveis de Ambiente (cy.env)
Objetivo: Isolar dados sensiveis, como senhas, fora do codigo fonte do teste utilizando as melhores praticas de seguranca.
1. Crie um arquivo chamado `cypress.env.json` na raiz do seu projeto.
2. Adicione um objeto JSON contendo as chaves `username` e `password` com as credenciais validas do OrangeHRM.
3. No seu script de teste, acesse a pagina de login.
4. Utilize o comando `Cypress.env('username')` e `Cypress.env('password')` dentro do comando `.type()` para preencher os campos.
5. Submeta o formulario e valide se o login foi realizado com sucesso.

### Exercicio 3: Criacao de um Comando Customizado de Login
Objetivo: Abstrair a logica repetitiva de autenticacao para reutilizacao em multiplos testes.
1. Navegue ate o arquivo `cypress/support/commands.js`.
2. Crie um comando customizado chamado `cy.login(usuario, senha)` utilizando `Cypress.Commands.add`.
3. Dentro da funcao do comando, adicione os passos para buscar os campos de usuario, senha e clicar no botao de login.
4. Em um arquivo de especificacao (spec), chame apenas `cy.login(Cypress.env('username'), Cypress.env('password'))`.
5. Valide no teste que o redirecionamento para `/dashboard` ocorreu com sucesso.

### Exercicio 4: Testando Links de Redes Sociais sem Sair da Pagina
Objetivo: Validar atributos de links externos de forma segura, sem tentar navegar para dominios de terceiros no teste.
1. Acesse a pagina inicial.
2. Localize os icones de redes sociais no rodape (LinkedIn, Facebook, Twitter, YouTube).
3. Escolha um dos icones e crie uma assercao utilizando `.should('have.attr', 'href')` seguida de `.and('include', 'linkedin.com')` (ou a rede correspondente).
4. Verifique tambem se a tag de ancora possui a propriedade `target` com o valor `_blank`, garantindo que o link abre em uma nova aba e nao substituira a sessao atual do usuario.
5. Repita o processo para os demais links iterando com o comando `.each()`.

### Exercício 5: Navegando para o módulo Admin

**Objetivo:** Automatizar a navegação utilizando o menu lateral.

1. Realize o login.
2. Clique na opção **Admin** do menu lateral.
3. Valide que a URL contém `/admin/viewSystemUsers`.
4. Confirme que o título **System Users** está visível.
5. Verifique que a tabela de usuários foi carregada.

### Exercício 6: Pesquisando um Usuário Existente

**Objetivo:** Utilizar os filtros disponíveis para localizar um usuário.

1. Realize o login.
2. Acesse o módulo **Admin**.
3. Localize o campo **Username**.
4. Digite `Admin`.
5. Clique em **Search**.
6. Aguarde a atualização da tabela.
7. Valide que pelo menos um registro foi encontrado.
8. Confirme que o usuário **Admin** aparece na tabela.

### Exercício 7: Limpando os Filtros de Pesquisa

**Objetivo:** Validar o funcionamento do botão **Reset**.

1. Realize o login.
2. Acesse o módulo **Admin**.
3. Informe qualquer valor no campo **Username**.
4. Clique em **Search**.
5. Clique no botão **Reset**.
6. Valide que o campo foi limpo.
7. Confirme que a tabela voltou ao estado inicial.

### Exercício 8: Pesquisando um Usuário Inexistente

**Objetivo:** Validar o comportamento da aplicação quando não existem resultados.

1. Realize o login.
2. Acesse o módulo **Admin**.
3. Informe `usuario123456` no campo **Username**.
4. Clique em **Search**.
5. Aguarde a pesquisa finalizar.
6. Valide a exibição da mensagem **No Records Found**.
7. Confirme que nenhuma linha foi exibida na tabela.

### Exercício 9: Navegando para o módulo PIM

**Objetivo:** Validar a navegação entre módulos.

1. Realize o login.
2. Clique na opção **PIM** no menu lateral.
3. Valide que a URL contém `/pim/viewEmployeeList`.
4. Verifique que o título **Employee Information** está visível.
5. Confirme que a tabela de funcionários foi carregada.

### Exercício 10: Pesquisando um Funcionário pelo Nome

**Objetivo:** Automatizar pesquisas utilizando campos com autocomplete.

1. Realize o login.
2. Acesse o módulo **PIM**.
3. Localize o campo **Employee Name**.
4. Digite parte do nome de um funcionário.
5. Aguarde a lista de sugestões aparecer.
6. Selecione uma das sugestões.
7. Clique em **Search**.
8. Valide que a pesquisa retornou pelo menos um funcionário.

### Exercício 11: Pesquisando Funcionário pelo Employee Id

**Objetivo:** Utilizar outro filtro disponível na tela de funcionários.

1. Realize o login.
2. Acesse o módulo **PIM**.
3. Informe um **Employee Id** existente.
4. Clique em **Search**.
5. Aguarde o carregamento da pesquisa.
6. Verifique que apenas um funcionário foi retornado.
7. Confirme que o Employee Id pesquisado aparece na tabela.

### Exercício 12: Validando o Botão Reset no módulo PIM

**Objetivo:** Garantir que todos os filtros sejam limpos corretamente.

1. Realize o login.
2. Acesse o módulo **PIM**.
3. Preencha um ou mais filtros.
4. Clique em **Search**.
5. Clique no botão **Reset**.
6. Valide que todos os campos ficaram vazios.
7. Confirme que a listagem completa voltou a ser exibida.

### Exercício 13: Automatizando um Fluxo Completo

**Objetivo:** Executar um fluxo completo simulando a navegação de um usuário.

1. Realize o login.
2. Acesse o módulo **Admin**.
3. Pesquise pelo usuário `Admin`.
4. Valide o resultado encontrado.
5. Navegue para o módulo **PIM**.
6. Pesquise um funcionário pelo nome.
7. Valide que a pesquisa retornou resultados.
8. Retorne ao **Dashboard** utilizando o menu lateral.
9. Verifique que a URL contém `/dashboard`.
10. Confirme que o título **Dashboard** está visível.
11. Finalize validando que o menu lateral permaneceu funcional durante toda a navegação.

---
### Fluxo Completo de Administração de Usuários e Funcionários

**Objetivo:** Automatizar um fluxo completo da aplicação, validando autenticação, navegação entre módulos, pesquisa, limpeza de filtros, persistência da sessão e funcionamento da interface durante toda a jornada do usuário.

### Cenário

Você foi contratado para validar um dos principais fluxos utilizados pela equipe de Recursos Humanos. O objetivo é garantir que um administrador consiga navegar pelos módulos da aplicação, realizar consultas e retornar ao Dashboard sem que ocorram falhas durante o processo.

### Requisitos

1. Acesse a página de login utilizando `cy.visit()`.
2. Realize o login utilizando:

   * **Usuário:** `Admin`
   * **Senha:** `admin123`
3. Valide que a URL contém `/dashboard`.
4. Verifique que o título **Dashboard** está visível.
5. Confirme que o menu lateral foi carregado corretamente.
6. Clique na opção **Admin**.
7. Valide que a URL contém `/admin/viewSystemUsers`.
8. Pesquise pelo usuário `Admin`.
9. Valide que o usuário foi encontrado na tabela.
10. Clique em **Reset** e confirme que o campo **Username** foi limpo.
11. Realize uma nova pesquisa utilizando um usuário inexistente (`usuario123456`).
12. Valide que a mensagem **No Records Found** é exibida.
13. Clique novamente em **Reset**.
14. Navegue para o módulo **PIM** utilizando o menu lateral.
15. Valide que a URL contém `/pim/viewEmployeeList`.
16. Pesquise um funcionário utilizando o campo **Employee Name**.
17. Aguarde o autocomplete aparecer e selecione uma das opções.
18. Clique em **Search**.
19. Valide que a pesquisa retornou pelo menos um funcionário.
20. Clique em **Reset** e confirme que todos os filtros foram limpos.
21. Utilize o menu lateral para retornar ao **Dashboard**.
22. Valide que a URL voltou para `/dashboard`.
23. Abra o menu do usuário (canto superior direito).
24. Verifique que as opções **About**, **Support**, **Change Password** e **Logout** estão disponíveis.
25. Feche o menu e confirme que a aplicação continua funcional.
26. Acesse novamente o módulo **Admin** e valide que a sessão do usuário continua autenticada (sem redirecionamento para a tela de login).
27. Por fim, realize **Logout**.
28. Valide que a aplicação retornou para a tela de login.
29. Tente acessar diretamente a URL `/dashboard` utilizando `cy.visit()`.
30. Confirme que o sistema redireciona automaticamente para a tela de login, comprovando que a sessão foi encerrada corretamente.

